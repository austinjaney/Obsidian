# Replacing Windows File Servers with CentOS7
#sysadmin #linux #activedirectory #centOS7

After a fair amount of trial and error I finally have a process that’s working well for me.  This is in no way a comprehensive guide on using SSSD with Samba to authenticate active directory users/groups to file shares but its a great start and is working well in my lab.  Many thanks to all those who contributed to articles in the helpful resources list at the bottom.

## Part 1: Install and configure SSSD
### Packages needed for SSSD to work correctly

```bash
yum install realmd sssd adcli oddjob oddjob-mkhomedir samba-common-tools net-tools ntpdate ntp
```

### Network Configuration
make sure you have a network connection, if you installed the above packages then you should be good.
Edit your network configuration:

```bash
vi /etc/sysconfig/network
Hostname=centoshostname.addomainname.tld
```

Edit your hosts file:

```bash
vi /etc/hosts
192.168.1.2 centoshostname.addomainname.tld
```

Restart networking

```bash
/etc/init.d/network restart
```

### Setup System Time
```bash
systemctl enable ntpd.service
ntpdate yourdomaincontroller.yourdomain.tld
systemctl start ntpd.service
```> Note: some have noted that in order for things to work right you might need to add your DC as a server entry to _etc_ntp.conf, I have not yet needed to do this.

### Join The Domain
```bash
sudo realm join -v -U domainuser addomainname.tld
```

You can use either the ID command against a user or use realm list to discover if you have joined the domain.

### Considerations
1. Once you are domain joined anyone on the domain can SSH into the joined server.
2. You may want to lock down your sudoers policy

### SSH Config
In order to limit what users are allowed to login to the newly joined server you will want to edit your ssh config _etc_ssh/sshd_config

Add the lines:

```bash
AllowGroups groupname@domain.whatever
```

Adding an ad group to control ssh permissions is a good idea, if you were to add the group ssh-users in AD you would add the line:

```bash
AllowGroups ssh-users@domain.whatever
```> Note: don’t assume group nesting will work, SSSD only looks at the immediate users of a group.
> Note: Doing this will explicitly allow only members of domain group you listed to log in.  

### sudoers file
this is not the best or least privilege way to do this but it is the way that will allow you to control everything in AD, create a group in AD that you want to give sudoers rights to and add the following line to your sudoers file on your newly joined server.
Traditionally, the visudo command opens the _etc_sudoers file with the vi text editor.

```bash
%groupname@ADDOMAIN.TLD ALL=(ALL:ALL) ALL
```> Note: caps may be required for the domain name.

## Part 2: Install and configure SAMBA
### Install SAMBA
```bash
Yum install samba
```

### Make sure samba can talk threw the firewall
```bash
firewall-cmd --permanent --add-service=samba
firewall-cmd --reload
```

### smb.conf working example
The following samba config file was pulled from a working server.

```bash
# See smb.conf.example for a more detailed config file or
# read the smb.conf manpage.
# Run 'testparm' to verify the config is correct after
# you modified it.
[global]
        workgroup = YOURDOMAINNAMEWITHNOTLD
        server string = Samba Server Version %v
        encrypt passwords = yes
        security = ads
        realm = REPLACEWITHYOURDOMAINNAME
        passdb backend = tdbsam
        printing = cups
        printcap name = cups
        load printers = yes
        cups options = raw
        kerberos method = secrets and keytab
        load printers = no
        cups options = raw
        printcap name = /dev/null
        log file = /var/log/samba/log.%m
        max log size = 50
#Test fix for idmap bug
        idmap config * : backend = tdb
        idmap config * : range = 300000-400000
[home directory]
        path = /home/%u
        comment = Home Directories
        guest ok = no
        browseable = yes
        read only = no
        inherit acls = yes
        inherit permissions = yes
        valid users = @“SOMEGROUP@YOURDOMAIN.TLD"
        admin users = @"SOMEGROUP@YOURDOMAIN.TLD"
[printers]
        comment = All Printers
        path = /var/tmp
        printable = Yes
        create mask = 0600
        browseable = No
[print$]
        comment = Printer Drivers
        path = /var/lib/samba/drivers
        write list = @printadmin root
        force group = @printadmin
        create mask = 0664
        directory mask = 0775
```

An example of what look to me like some sane defaults from /http://www.hexblot.com/blog/centos-7-active-directory-and-samba/ includes:

```bash
[global]
workgroup = MYDOMAINLOCAL
server string = Samba Server Version %v

# Add the IPs / subnets allowed acces to the server in general.
# The following allows local and 10.0.*.* access
hosts allow = 127. 10.0.

# log files split per-machine:
log file = /var/log/samba/log.%m
# enable the following line to debug:
# log level =3
# maximum size of 50KB per log file, then rotate:
max log size = 50

# Here comes the juicy part!
security = ads
encrypt passwords = yes
passdb backend = tdbsam
realm = MYDOMAIN.LOCAL

# Not interested in printers
load printers = no
cups options = raw

# This stops an annoying message from appearing in logs
printcap name = /dev/null
```

Now that samba is setup to share /home you’ll need to edit permissions on /home so users can access their home folders.  In the case of active directory domain home folders using “domain /users@yourdomain.tld/” should provide a good option.

```bash
chown root:"adgroupyoumade@yourdomain.tld" /home
chmod 0770 /home
```

### Note about SELinux
If you haven’t disabled it (which you probably shouldn’t) Upon finishing up and setting permissions you might find that you can’t access your shares, it might be SELinux. You either need to
> (Please don’t) disable it completely (by setting SELINUX=disabled in _etc_sysconfig/selinux ) or  
> enter the following command for each share you make:  

```bash
chcon -t samba_share_t /var/myshare
```

To share out home directories you will need to run

```bash
setsebool -P samba_enable_home_dirs on
```

### Enable and Start SAMBA
```bash
systemctl enable smb.service
systemctl start smb.service
```

Congrats! You should now be able to authenticate to your samba file shares using active directory authentication!

### Helpful resources
Securing samba shares: /http://linux-training.be/networking/ch21.html/
Notes on SE Linus and best practice: /https://wiki.centos.org/HowTos/SetUpSamba/
Notes on integrating with AD (huge thanks to Hexblot) /http://www.hexblot.com/blog/centos-7-active-directory-and-samba/