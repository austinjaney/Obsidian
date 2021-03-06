# Zulip Ops Doc
#northcoast #sysadmin #ubuntu #linux #docs 

Zulip Supplies their own Ops Doc here: <https://zulip.readthedocs.io/en/latest/production/maintain-secure-upgrade.html>

We have disabled unattended updates to our Zulip server container.

### Restarting Zulip
```bash
su zulip -c '/home/zulip/deployments/current/scripts/restart-server'
```

### Tailing Postfix Logs
```bash
cd /var/log
tail -f mail.log
```

### Applying system updates
The Zulip upgrade script will automatically run apt-get update and then apt-get upgrade, to make sure you have any new versions of dependencies (this will also update system packages). We assume that you will install security updates from apt regularly, according to your usual security practices for a production server.

If you’d like to minimize downtime when installing a Zulip server upgrade, you may want to do an apt-get upgrade (and then restart the server and check everything is working) before running the Zulip upgrade script.

There’s one apt package to be careful about: upgrading postgresql while the server is running may result in an outage (basically, postgresql might stop accepting new queries but refuse to shut down while waiting for connections from the Zulip server to shut down). While this only happens sometimes, it can be hard to fix for someone who isn’t comfortable managing a postgresqldatabase [1]. You can avoid that possibility with the following procedure (run as root):

```bash
apt-get update
supervisorctl stop all
apt-get upgrade -y
supervisorctl start all
```

[1] If this happens to you, just stop the Zulip server, restart postgres, and then start the Zulip server again, and you’ll be back in business.

#### In some situations it might be best to only install security updates to do this
check for security updates:
```bash
apt-get -s dist-upgrade| grep "^Inst" | grep -i security
supervisorctl stop all
```

Install listed security updates:
```bash
apt-get -s dist-upgrade | grep "^Inst" | grep -i securi | awk -F " " {'print $2'} | xargs apt-get install
supervisorctl start all
```

##### Network Configuration
```bash
root@zulip:~# cat /etc/netplan/50-cloud-init.yaml
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      dhcp6: no
      tmu: 1472
      addresses: [209.242.155.200/26]
      gateway4: 209.242.155.193
      nameservers:
        addresses: [9.9.9.9,149.112.112.112]
```

* Netplan apply will apply the network configuration

### Admin Notes about our Config
#### By default tulip keeps uploaded documents in:
```
#   https://zulip.readthedocs.io/en/latest/production/upload-backends.html
LOCAL_UPLOADS_DIR = "/home/zulip/uploads"
#S3_AUTH_UPLOADS_BUCKET = ""
#S3_AVATAR_BUCKET = ""
#S3_REGION = ""
```
Set max file size:
```
# uploads, so browsers will crash if you try uploading larger files.
MAX_FILE_UPLOAD_SIZE = 25
```

#### Disable Jitsi
```
# Controls the Jitsi video call integration.  By default, the
# integration uses the SaaS meet.jit.si server.  You can specify
# your own Jitsi Meet server, or if you'd like to disable the
# integration, set JITSI_SERVER_URL = None.
#JITSI_SERVER_URL = 'jitsi.example.com'
```

#### Postfix Configuration
Postfix master.cf change
This change was made so that postfix would stop attempting to deliver bounced messages and discard them.
https://serverfault.com/questions/824163/disable-the-sending-of-undeliverable-mail-returned-to-sender-in-postfix

Open _etc_postfix/master.cf file and find the following line:
```
bounce unix - - n - 0 bounce
```

Change it to this:

```
bounce unix - - n - 0 discard
```

Next, restart Postfix.
Please be advised that you will see an error in your logs whenever Postfix runs the discard handler instead of the bounce handler. The errors will look similar to this:

Dec 15 16:07:40 websrv1 postfix/discard[15220]: warning: unexpected attribute nrequest from bounce socket (expecting: flags)

Dec 15 16:07:40 websrv1 postfix/discard[15220]: warning: >deliver_request_get: error receiving common attributes

```bash
root@zulip:~# cat /etc/postfix/master.cf
#
# Postfix master process configuration file.  For details on the format
# of the file, see the master(5) manual page (command: "man 5 master").
#
# Do not forget to execute "postfix reload" after editing this file.
#
# ==========================================================================
# service type  private unpriv  chroot  wakeup  maxproc command + args
#               (yes)   (yes)   (yes)   (never) (100)
# ==========================================================================
smtp      inet  n       -       -       -       -       smtpd
#smtp      inet  n       -       -       -       1       postscreen
#smtpd     pass  -       -       -       -       -       smtpd
#dnsblog   unix  -       -       -       -       0       dnsblog
#tlsproxy  unix  -       -       -       -       0       tlsproxy
#submission inet n       -       -       -       -       smtpd
#  -o syslog_name=postfix/submission
#  -o smtpd_tls_security_level=encrypt
#  -o smtpd_sasl_auth_enable=yes
#  -o smtpd_client_restrictions=permit_sasl_authenticated,reject
#  -o milter_macro_daemon_name=ORIGINATING
#smtps     inet  n       -       -       -       -       smtpd
#  -o syslog_name=postfix/smtps
#  -o smtpd_tls_wrappermode=yes
#  -o smtpd_sasl_auth_enable=yes
#  -o smtpd_client_restrictions=permit_sasl_authenticated,reject
#  -o milter_macro_daemon_name=ORIGINATING
#628       inet  n       -       -       -       -       qmqpd
pickup    fifo  n       -       -       60      1       pickup
cleanup   unix  n       -       -       -       0       cleanup
qmgr      fifo  n       -       n       300     1       qmgr
#qmgr     fifo  n       -       n       300     1       oqmgr
tlsmgr    unix  -       -       -       1000?   1       tlsmgr
rewrite   unix  -       -       -       -       -       trivial-rewrite
bounce    unix  -       -       n       -       0       discard
defer     unix  -       -       -       -       0       bounce
trace     unix  -       -       -       -       0       bounce
verify    unix  -       -       -       -       1       verify
flush     unix  n       -       -       1000?   0       flush
proxymap  unix  -       -       n       -       -       proxymap
proxywrite unix -       -       n       -       1       proxymap
smtp      unix  -       -       -       -       -       smtp
relay     unix  -       -       -       -       -       smtp
#       -o smtp_helo_timeout=5 -o smtp_connect_timeout=5
showq     unix  n       -       -       -       -       showq
error     unix  -       -       -       -       -       error
retry     unix  -       -       -       -       -       error
discard   unix  -       -       -       -       -       discard
local     unix  -       n       n       -       -       local
virtual   unix  -       n       n       -       -       virtual
lmtp      unix  -       -       -       -       -       lmtp
anvil     unix  -       -       -       -       1       anvil
scache    unix  -       -       -       -       1       scache
#
# ====================================================================
# Interfaces to non-Postfix software. Be sure to examine the manual
# pages of the non-Postfix software to find out what options it wants.
#
# Many of the following services use the Postfix pipe(8) delivery
# agent.  See the pipe(8) man page for information about ${recipient}
# and other message envelope options.
# ====================================================================
#
# maildrop. See the Postfix MAILDROP_README file for details.
# Also specify in main.cf: maildrop_destination_recipient_limit=1
#
maildrop  unix  -       n       n       -       -       pipe
  flags=DRhu user=vmail argv=/usr/bin/maildrop -d ${recipient}
#
# ====================================================================
#
# Recent Cyrus versions can use the existing "lmtp" master.cf entry.
#
# Specify in cyrus.conf:
#   lmtp    cmd="lmtpd -a" listen="localhost:lmtp" proto=tcp4
#
# Specify in main.cf one or more of the following:
#  mailbox_transport = lmtp:inet:localhost
#  virtual_transport = lmtp:inet:localhost
#
# ====================================================================
#
# Cyrus 2.1.5 (Amos Gouaux)
# Also specify in main.cf: cyrus_destination_recipient_limit=1
#
#cyrus     unix  -       n       n       -       -       pipe
#  user=cyrus argv=/cyrus/bin/deliver -e -r ${sender} -m ${extension} ${user}
#
# ====================================================================
# Old example of delivery via Cyrus.
#
#old-cyrus unix  -       n       n       -       -       pipe
#  flags=R user=cyrus argv=/cyrus/bin/deliver -e -m ${extension} ${user}
#
# ====================================================================
#
# See the Postfix UUCP_README file for configuration details.
#
uucp      unix  -       n       n       -       -       pipe
  flags=Fqhu user=uucp argv=uux -r -n -z -a$sender - $nexthop!rmail ($recipient)
#
# Other external delivery methods.
#
ifmail    unix  -       n       n       -       -       pipe
  flags=F user=ftn argv=/usr/lib/ifmail/ifmail -r $nexthop ($recipient)
bsmtp     unix  -       n       n       -       -       pipe
  flags=Fq. user=bsmtp argv=/usr/lib/bsmtp/bsmtp -t$nexthop -f$sender $recipient
scalemail-backend unix  -       n       n       -       2       pipe
  flags=R user=scalemail argv=/usr/lib/scalemail/bin/scalemail-store ${nexthop} ${user} ${extension}
mailman   unix  -       n       n       -       -       pipe
  flags=FR user=list argv=/usr/lib/mailman/bin/postfix-to-mailman.py
  ${nexthop} ${user}
zulip unix  -       n       n       -       -       pipe
  flags= user=zulip argv=/home/zulip/deployments/current/scripts/lib/email-mirror-postfix -r ${original_recipient}
```

### Deployment Process
Build the container

```
lxc launch ubuntu:18.04 zulip
```

Setup Networking with Netplan

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

```
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
 version: 2
 renderer: networkd
 ethernets:
   eth0:
     dhcp4: no
     dhcp6: no
     addresses: [172.16.1.6/24]
     gateway4: 172.16.1.1
     nameservers:
       addresses: [8.8.8.8,8.8.4.4]
```

Download Zulip:

```bash
cd $(mktemp -d)
wget https://www.zulip.org/dist/releases/zulip-server-latest.tar.gz
tar -xf zulip-server-latest.tar.gz
```

Update Ubuntu:

```bash
sudo apt update && sudo apt upgrade -y
```

Install Zulip:

```bash
sudo -s ./zulip-server-*/scripts/setup/install --certbot \--email=ajaney@northcoastchurch.com --hostname=zulip.northcoastchurch.com
```

##### Known Bugs
There is a known bug in how Bionic+ configures Postfix, Our correct Postfix [main.cf](http://main.cf) should read:

* If the line ::smtpd_relay_restrictions = permit_mynetworks, reject_unauth_destination:: does not exist email won’t work. 
	* I discovered that from: [GitHub](https://github.com/plone/ansible-playbook/commit/5f0309834bbdf0489dd343449c35a98f6fe21284)
* Running: _home_zulip_deployments_current_scripts_zulip-puppet-apply 
	* Will cause this bug and erase the highlighted line, Zulip support says that this bug should be corrected in the next release.

```bash
root@zulip:~# cat /etc/postfix/main.cf
# This file is managed by puppet; local changes will be overridden.


smtpd_banner = $myhostname ESMTP $mail_name (Zulip Voyager)
biff = no


# appending .domain is the MUA's job.
append_dot_mydomain = no


readme_directory = no


# TLS parameters
smtpd_tls_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
smtpd_tls_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
smtpd_use_tls=yes
smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache


myhostname = zulip
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
transport_maps = hash:/etc/postfix/transport
myorigin = /etc/mailname
mydestination = localhost, zulip.northcoastchurch.com
relayhost =
mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128
mailbox_size_limit = 0
recipient_delimiter = +
inet_interfaces = all
inet_protocols = all
smtpd_relay_restrictions = permit_mynetworks, reject_unauth_destination


# This enables us to do a catchall
virtual_alias_maps = regexp:/etc/postfix/virtual


# Needed to accept mail with percents without trying to interpret as
# percent-style routing.
allow_percent_hack = no
allow_untrusted_routing = yes
strict_rfc821_envelopes = no
```
