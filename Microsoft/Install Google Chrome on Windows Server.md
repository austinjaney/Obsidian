# Install google chrome one windows server with Powershell
#powershell #windows #support 

The following command is a one shot to install chrome on a windows server.

```powershell
Invoke-WebRequest -Uri "https://dl.google.com/chrome/install/latest/chrome_installer.exe" -OutFile chrome_installer.exe ; .\chrome_installer.exe
```