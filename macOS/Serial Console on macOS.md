# Serial Console on macOS
#macos #network 

Open Terminal
```bash
cd /dev/
ls -l | grep usb
```

Look for the tty.usbserial-*
```bash
screen tty.usbserial-* 9600
```

To exit hold ctrl+a and then ctrl+\