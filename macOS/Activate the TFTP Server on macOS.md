# Activate the TFTP Server on macOS
#macos

Activate the tftp server on your Mac:

To change the properties, edit the file

```bash
/System/Library/LaunchDaemons/tftp.plist
```

The default directory is _private_tftpboot.

Make this directory accessible for everybody.

```bash
sudo chmod 777 /private/tftpboot
```

and start it with

```bash
sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist
```

If you want to stop the daemon, do

```bash
sudo launchctl unload /System/Library/LaunchDaemons/tftp.plist
```