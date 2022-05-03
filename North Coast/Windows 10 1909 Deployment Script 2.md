# Windows 10 1909 Deployment Script
#sysadmin/microsoft/windows/powershell #powershell

Decrapify Windows only (block)
Running this code block from audit mode will run the windows decrapification script and then shut the system down, on next boot the system will boot into the out of box experence.

```powershell
Update-MpSignature
```

```powershell
Get-Process sysprep | % { $_.CloseMainWindow() } ; mkdir C:\Users\Public\Downloads\Deploy ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/decrapify1903.ps1" -Destination C:\Users\Public\Downloads\Deploy\decrapify1903.ps1 ; cd "C:\Users\Public\Downloads\Deploy" ; PowerShell.exe -ExecutionPolicy Bypass -File .\decrapify1903.ps1 -onedrive -clearstart -leaveservices -leavetasks -xbox -nolog ; C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown /oobe
```

##### Code Blocks
```powershell
# because we are in audit mode sysprep is already open, closing it now prevents issues later in the script where we need to run it.
Get-Process sysprep | % { $_.CloseMainWindow() }
```

```powershell
# make the directory where our scripts and deployment tools will be downloaded
mkdir C:\Users\Public\Downloads\Deploy
```

```powershell
# cd to our download directory, we could target the script from its full path but the more lines we add the more cluttered it becomes.
cd "C:\Users\Public\Downloads\Deploy"
```

```powershell
# Download and install office 2019
Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/config-Office2019-Standard-x64.xml" ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/setup.exe" ;  .\setup.exe /configure .\config-Office2019-Standard-x64.xml
```

```powershell
# Download and install google chrome
Start-BitsTransfer "https://dl.google.com/chrome/install/latest/chrome_installer.exe" ; .\chrome_installer.exe
```

```powershell
# Download and install Acrobat Reader
Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/AcroRdrDC1901220036_en_US.exe" ; .\AcroRdrDC1901220036_en_US.exe /sAll /msi EULA_ACCEPT=YES /qn
```

```powershell
# kickoff the download using bitstransfer
Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/decrapify1903.ps1"
```

```powershell
# time to decrapify! (and do some clean up, we will delete the stuff we downloaded)
PowerShell.exe -ExecutionPolicy Bypass -File .\decrapify1903.ps1 -onedrive -clearstart -leaveservices -leavetasks -xbox -nolog ; C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown /oobe
```

### Deploy!
We are finally ready to run our collection of scripts as a code block in audit mode.

```powershell
Update-MpSignature
```

```powershell
Get-Process sysprep | % { $_.CloseMainWindow() } ; mkdir C:\Users\Public\Downloads\Deploy ; cd "C:\Users\Public\Downloads\Deploy" ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/config-Office2019-Standard-x64.xml" ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/setup.exe" ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/AcroRdrDC1901220036_en_US.exe" ; Start-BitsTransfer "https://dl.google.com/chrome/install/latest/chrome_installer.exe" ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/decrapify1903.ps1" ; .\setup.exe /configure .\config-Office2019-Standard-x64.xml ; .\AcroRdrDC1901220036_en_US.exe /sAll /msi EULA_ACCEPT=YES /qn ; .\chrome_installer.exe ; PowerShell.exe -ExecutionPolicy Bypass -File .\decrapify1903.ps1 -onedrive -clearstart -leaveservices -leavetasks -xbox -nolog ; C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown /oobe
```

Activate Office 2019 from powershell

```powershell
cscript "C:\Program Files\Microsoft Office\Office16\OSPP.vbs" /inpkey:xxxxx-xxxxx-xxxxx-xxxxx-xxxxx
```