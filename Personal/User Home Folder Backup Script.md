# User Home Folder Backup Script
#sysadmin #powershell #windows 

```powershell
### Azulpine User Home Folder Backup Script
### Changes in this version: target of backups was changed so that the backups now point to the users home directory
### Created by Austin Janey on 6/13/17
### This script is designed to backup the users home folder on logon and send the logs of the backup job to the backup location's log folder.

### Creating the file in the users shared folder so that we have a place to dump the log files
mkdir \\192.168.88.3\$env:UserName\Backup
mkdir \\192.168.88.3\$env:UserName\Backup\logs

### Editing this determines what is backed up
robocopy "C:\Users\$env:UserName" "\\192.168.88.3\$env:UserName\Backup" /E /XA:SH /XD /log:\\192.168.88.3\$env:UserName\Backup\logs\newlogfile.txt "Appdata" "My Music" "PrintHood" "MY Pictures" "My Documents" "Recent" "Searches" "Saved Games" "Templates" "SendTo" "NetHood" "Local Settings" "My Videos" "Cookies" "Application Data" "Dropbox" "Start Menu" /XF /R:1 /W:5 *.pst *.vir *.js *.jar *.jse *.lnk *.LOG1 *.DAT

### Timestamps the log file produced by robocopy by renaming it with the timestamp
Rename-Item \\192.168.88.3\$env:UserName\Backup\logs\newlogfile.txt "$env:UserName-$((get-date).toString('backup_dd-MM-yyyy')).txt"

### moves the log to the logs folder in the U drive
Move-Item \\192.168.88.3\$env:UserName\Backup\logs\"$env:UserName-$((get-date).toString('backup_dd-MM-yyyy')).txt" \\192.168.88.3\$env:UserName\Backup\logs\
```