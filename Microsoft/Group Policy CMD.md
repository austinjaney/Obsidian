# Group Policy CMD
#sysadmin #cmd #windows

This command is suppose to be able to expire the computers kerberos ticket thus forcing the computer to get a new one to detect a change in OU.

```powershell
klist -li 0x3e7 purge
```

Getting the group policy results from a workstation through psexec

```powershell
gpresult /user User-Logged-In /scope computer /r
```

See remotely installed printers I use:

```powershell
wmic printer list brief
```

```powershell
wmic printer get name
```

This just shows a short list of printer attached to the system you run the command on. It will also show what computer a printer is connected to if there's a network printer.

You can also use this to get a very detailed list of configuration for each printer installed on a system:

```powershell
wmic printer list full
```

To output it to a text file, append this to the end of the command:
Example:

```powershell
wmic printer list brief >> c:\users\admin\documents\printerlist.txt
```

Info from Microsoft: https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh134826(v=ws.10)

Update group membership without reboot

Written by Oddvar Moe
I often add the servers computer account to an Active Directory Group. And as we all know you have to boot in order to update this group membership. Or maybe not?? In order to update group membership without rebooting you need psexec or another method of starting a CMD as system. Start psexec –s cmd . Verify using whoami command to see that you are running as system. Then you type: 

klist purge 
klist is a kerberos command for viewing and purging cached kerberos tickets. After you have purged it the membership will be updated the next time you try to access something that requires that membership.