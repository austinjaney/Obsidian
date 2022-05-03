# Install only security updates for Ubuntu
#sysadmin #linux 

check for security updates:

```bash
apt-get -s dist-upgrade| grep "^Inst" | grep -i security
```

Install listed security updates:

```bash
apt-get -s dist-upgrade | grep "^Inst" | grep -i securi | awk -F " " {'print $2'} | xargs apt-get install
```