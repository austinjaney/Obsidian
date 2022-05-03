# Flush DNS Cache in macOS
#macos 

Use the following 2 commands:
```bash
dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```