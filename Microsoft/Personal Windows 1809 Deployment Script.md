#sysadmin #powershell #windows

# Personal Windows 1809 Deployment Script

This Script is designed to take junk out of windows 10 1803 and 1809.  I wrote this to make my own deployments of windows 10 more clean for virtual and phisical systems. It pulls from my own public backblaze repository. 

Code Block for copy and paste

```powershell
Get-Process sysprep | % { $_.CloseMainWindow() } ; mkdir C:\Users\Public\Downloads\Deploy ; Start-BitsTransfer "https://f001.backblazeb2.com/b2api/v1/b2_download_file_by_id?fileId=4_zd379eed7baf39ec56c930614_f11201fc48a84f9b3_d20190320_m201708_c001_v0001090_t0025" -Destination C:\Users\Public\Downloads\Deploy\decrapify1809.ps1 ; cd "C:\Users\Public\Downloads\Deploy" ; PowerShell.exe -ExecutionPolicy Bypass -File .\decrapify1809.ps1 -onedrive -clearstart -leaveservices -leavetasks -xbox ; C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown /oobe
```

Breakdown of Script

```powershell
# Close out of sysprep
Get-Process sysprep | % { $_.CloseMainWindow() }

# Create a directory for the script to be downloaded to
mkdir C:\Users\Public\Downloads\Deploy

# Download the script
Start-BitsTransfer "https://f002.backblazeb2.com/b2api/v1/b2_download_file_by_id?fileId=4_z100c0220e8b69b686e920616_f118bb3581b671d65_d20190320_m162139_c002_v0001118_t0035" -Destination C:\Users\Public\Downloads\Deploy\decrapify1809.ps1

# CD to the deployment directory
cd "C:\Users\Public\Downloads\Deploy"

# Execute the script
PowerShell.exe -ExecutionPolicy Bypass -File .\decrapify1809.ps1 -onedrive -clearstart -leaveservices -leavetasks -xbox

# Run Sysprep and shut the system down so it boots back up into the out of box experence
C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown /oobe
```