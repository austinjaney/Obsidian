# Windows 10 1903 Deployment Process
#sysadmin #northcoast #windows #powershell

### Goals: 
1. Speed up the process of deploying windows 10 to different hardware platforms by making the configuration of the operating system automatic.
2. strip out default bloatware included with non pro versions of windows. 
3. Aid in the process of creating system images and remove dependency on cloning the drives of computers that have been setup.

There have been some changes to the way that this script must be automated, the most major of these changes is that windows now requires your antivirus signatures to be up to date before it will allow you to run Powershell scripts. 

#### What does the script do exactly?
1. Installs Office 2019 Standard using the Microsoft office deployment tool, 
2. Installs Google Chrome
3. Installs Adobe Acrobat Reader DC
4. Runs a Windows cleanup script with special modifications that remove all removable bloat from windows 10.
5. Runs Sysprep generalize and shuts the computer down, after which a system image can be created and used to provision windows on similar hardware or the computer can be setup and deployed to an end user.

#### Process for provisioning a new computer
1. Enable UEFI booting, Enable Secure boot, Disable legacy boot options.
2. Install windows 10 1903, upon being prompted by the out of box experience hit ctrl+shift+f3 to enter windows audit mode.  Windows will reboot and log you into the default Administrator account so you can make persistent changes to windows.
3. Open Powershell as an admin and run the following commands:
```powershell
Update-MpSignature
```

```powershell
Get-Process sysprep | % { $_.CloseMainWindow() } ; mkdir C:\Users\Public\Downloads\Deploy ; cd "C:\Users\Public\Downloads\Deploy" ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/config-Office2019-Standard-x64.xml" ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/setup.exe" ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/AcroRdrDC1901220036_en_US.exe" ; Start-BitsTransfer "https://dl.google.com/chrome/install/latest/chrome_installer.exe" ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/decrapify1903.ps1" ; .\setup.exe /configure .\config-Office2019-Standard-x64.xml ; .\AcroRdrDC1901220036_en_US.exe /sAll /msi EULA_ACCEPT=YES /qn ; .\chrome_installer.exe ; PowerShell.exe -ExecutionPolicy Bypass -File .\decrapify1903.ps1 -onedrive -clearstart -leaveservices -leavetasks -xbox -nolog ; C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown /oobe
```

4. the computer will shut down after completing sysprep and on next boot will boot back into the out of box experience.  Setup can now be finished as normal. The adm-it account and joining the system to the domain moving it to the correct OU.
5. Office 2019 Standard will need to be activated, this can be done by entering the key manually or running the following command from elevated command prompt or powershell, replace the xxxxx with your key:
```powershell
cscript "C:\Program Files\Microsoft Office\Office16\OSPP.vbs" /inpkey:xxxxx-xxxxx-xxxxx-xxxxx-xxxxx
```

## Detailed Breakdown of what is actually happening behind the scenes
The following code blocks have been concatenated into a block that can be easily pasted into powershell.  It is true that I could have put these into a powershell instead of joining them together as a long string but that would have been less transparent and no easier to use. 

##### Code Blocks
```powershell
# because we are in audit mode sysprep is already open, closing it now prevents issues later in the script where we need to run it again with command variables.
Get-Process sysprep | % { $_.CloseMainWindow() }
```

```powershell
# make the directory where our scripts and deployment tools will be downloaded this location will remain and be populated after windows is deployed.
mkdir C:\Users\Public\Downloads\Deploy
```

```powershell
# cd to our download directory, we could target the script from its full path but the more lines we add the more cluttered it becomes.
cd "C:\Users\Public\Downloads\Deploy"
```

```powershell
# Download and install office 2019, the setup.exe is the standard Microsoft one, the config contains what products need to be installed.
Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/config-Office2019-Standard-x64.xml" ; Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/setup.exe" ;  .\setup.exe /configure .\config-Office2019-Standard-x64.xml
```

```powershell
# Download and install the latest build of google chrome.
Start-BitsTransfer "https://dl.google.com/chrome/install/latest/chrome_installer.exe" ; .\chrome_installer.exe
```

```powershell
# Download and install Acrobat Reader, this is static and will need to be updated periodically, alternatively I could add a Powershell script that grabs the latest adobe update package and installs it automatically. 
Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/AcroRdrDC1901220036_en_US.exe" ; .\AcroRdrDC1901220036_en_US.exe /sAll /msi EULA_ACCEPT=YES /qn
```

```powershell
# Downloads the windows 10 1903 decrapification script and runs it with variables.  I tweaked this script a tad so that it would not break anything in windows.  It does not remove the windows store, xbox app or onedrive but does remove all the bloat from the start menu and prevents the install of automatic store apps like candycrush.  It leaves all default services alone and setup tasks.  I tweaked it this way deliberately so that it won't disturb the operation of windows in the long term. 
Start-BitsTransfer "https://f001.backblazeb2.com/file/azulpine-microsoft-deployment/decrapify1903.ps1" ; PowerShell.exe -ExecutionPolicy Bypass -File .\decrapify1903.ps1 -onedrive -clearstart -leaveservices -leavetasks -xbox -nolog
```

```powershell
# Run Sysprep generalize and shutdown, on next boot boot directly to the windows out of box experience setup. 
C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown /oobe
```
