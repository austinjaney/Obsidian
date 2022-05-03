# The Scroll of Utility!
#sysadmin #support 

You wander into a dark cave and encounter a grumpy old computer, you look to your left and see a chest, you run to it open it and find the scroll of utility! A collection of helpful system tools for helping helpdesk technicians do their jobs.

### Windows Utilities
[TFC](https://f001.backblazeb2.com/file/scroll-of-utility/TFC.exe) MD5: 788fcddd88240a85039f7f561093b118

> The temp file cleaner by old-timer is a classic utility designed to clean all the old temp files off of windows systems, works with Windows xp - Windows 10

[Take Ownership](https://f001.backblazeb2.com/file/scroll-of-utility/TakeOwnership.zip) MD5: 38a8674b9bb64a27ec999fcc9e3df662

> An old registry hack that enables a take ownership right click option, useful for when you’re stuck with a file you can’t seem to change the permissions of even though you have admin access.

[hpflash1](https://f001.backblazeb2.com/file/scroll-of-utility/hpflash1.zip) MD5: e30ffd26b45c78303085dc4f35a24a80

> The HP flash utility, great for making DOS boot drives

[DoubleDriver](https://f001.backblazeb2.com/file/scroll-of-utility/doubledriver.zip) MD5: 98f948a5806cf6d84bfb2dabc8c48a95

> Double Driver, the unsung hero of printer migrations and new system builds.  This utility can suck the drivers right out of an existing windows install and throw them directly at a new one. 

[Putty 0.72 32bit exe](https://f001.backblazeb2.com/file/scroll-of-utility/putty.exe) MD5: d9e402762e546c0046ad4748778472e1

[https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) 

> For all your SSH, Serial and Telnetting needs.

*Sysinternals Suite as of June 28 2019*
MD5: 0bf21987bcfcce217a4fbda399ce2b63
 [https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)

### Windows CLI things

Get the service tag of dell PC's from Command Prompt or Powershell

```powershell
wmic bios get serialnumber
```

Expire a computers kerberos ticket thus forcing the computer to get a new one this helps windows detect a change in AD OU's without rebooting so that you can run gpupdate /force without needing to reboot. Useful for systems that cant be shut down but do need to be moved in AD.

```powershell
klist -li 0x3e7 purge
```

Getting the group policy results from a workstation through psexec, replace $User-Logged-In with a logged in user.

```powershell
gpresult /user $User-Logged-In /scope computer /r
```
