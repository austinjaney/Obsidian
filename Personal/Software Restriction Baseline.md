# Software Restriction Baseline
#personal #blog

A guide for setting up Software Restrictions in Group Policy.

Under Enforcement Properties set "All software files except libraries (such as DLLs)", "All users except local administrators" (which will allow members of the local administrators group to bypass the policy completely) and ignore certificate rules unless you are planning to whitelist software via certificates, this can be handy for allowing user to install some programs to %appdata% such as slack, and or certain video conferencing software.

Under Designated File Types remove .lnk files, leaving this option enabled can cause start menu items to stop working as well as all shortcuts to exe files which are now pervasive through the windows operating system. This is mentioned in the NSA document but they list making a rule to allow it, other sources recommend removing it from the designated file types list which seems to be the correct way to do this.
Under Security Levels set the policy to Disallow, this will prevent software from running regardless of the access rights of the user.

##### Paths the NSA recommends restricting
These have been taken from the NSA document: https://apps.nsa.gov/iaarchive/library/reports/application-whitelisting-using-srp.cfm

```powershell
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\Debug
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\PCHEALTH\ERRORREP
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\Registration
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\System32\catroot2
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\System32\com\dmp
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\System32\FxsTmp
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\System32\spool\drivers\color
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\System32\spool\PRINTERS
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\System32\spool\SERVERS
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\System32\Tasks
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\SysWOW64\com\dmp
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\SysWOW64\FxsTmp
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\SysWOW64\Tasks
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\Tasks
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\Temp
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRoot%\tracing
```

##### Paths the NSA recommends allowing
In the original document there is an invisible space right before "Windows" to be cautious of the below line has been corrected.

```powershell
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\ProgramFilesDir (x86)%
```

as well as sysvol so any logon scripts you have will still run, not adding this will break any logon scripts you have.

```powershell
\\%USERDNSDOMAIN%\Sysvol\
```

An alternate value for the x86 directory might be needed in certain versions of windows, it is not needed for windows 10. to add if the x86 exception listed by the NSA is causing difficulties adding the below line might resolve them. This should not be necessary in most cases.

```powershell
%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\ProgramW6432Dir%
```

##### Blocking The Windows Store and Xbox apps in Windows 10
Windows Store, blocking this will disable users from launching the windows store and thus prevent users from installing apps from it.

```powershell
%programfiles%\WindowsApps\Microsoft.WindowsStore*
```

Xbox Apps, Windows 10 ships with a couple different xbox applications, removing these is problematic but blocking them from running is not. This will prevent users from downloading PC games or connecting to and streaming from xbox systems on the network or outside of it. (Microsoft is adding an ability to connect to a home xbox in a future xbox release.)

```powershell
%programfiles%\WindowsApps\Microsoft.Xbox*
```

##### Whitelisting modern applications
Some windows applications such as OneDrive run from appdata, as such you may need to whitelist additional locations so that these applications can function properly, this also goes for windows store applications.  In the case of OneDrive the path is

```powershell
%localappdata%\Microsoft\Onedrive\
```

Applications installed from the windows app store (if you have chosen not to block them) will install to the directory C:\Program Files\WindowsApps whitelisting the specific directory can allow these to run, windows will allow you to use wildcards in the path names so for example

```powershell
C:\Program Files\WindowsApps\spo*
```
Will allow any app contained within any folder under WindowsApps that begins with "spo" to run, in this example spotify as installed from the windows app store would run.  

#### Other Considerations

- Helpful Notes from Server Fault: https://serverfault.com/questions/447078/applocker-vs-software-restriction-policy#711558
- Microsofts documentation on Software Restriction Policies: https://docs.microsoft.com/en-us/windows-server/identity/software-restriction-policies/administer-software-restriction-policies