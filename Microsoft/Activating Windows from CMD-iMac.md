# Activating Windows from CMD
#sysadmin #cmd #powershell #windows

There seems to be a bug in server 2019 where you can only active windows with your key by using the command line like so.

```
C:\windows\system32\slmgr.vbs /ipk your-key-here
```

If you download a copy of server 2012r2 from Microsoft's eval center you can upgrade to a full copy of server 2012r2 standard. 
In powershell you will need to run the following commands:

```powershell
DISM /online /Get-CurrentEdition
```

Make note of the edition ID, in my case I was using server standard and it listed ServerStandardEval. ServerStandard is server standard.

```powershell
DISM /online /Set-Edition:ServerStandard /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula
```

> Replace ServerStandard with your desired edition and product key with the one you intend to use, windows will reboot and you should see that you’re now running your desired version of windows server. You can now activate windows normally using the system and security system dialogue.