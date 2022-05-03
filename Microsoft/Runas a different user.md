# Runas a different user
#windows #cmd #support 

On windows you can run exe files and other files as a different user or an admin user.

Hold shift and right click then select "Run as a different user" in the GUI

Using Command Prompt

```
runas /user:Bob “C:\Windows\notepad.exe”
```

Run as an admin by highlighting the application you want to run and using ctrl+shift+enter this will bring up a UCA prompt.