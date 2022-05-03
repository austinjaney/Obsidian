# Find Host from Guest
#hyper-v #powershell #sysadmin 

Running this in the guest powershell will allow you to find the hostname of the host. 

```powershell
(get-item "HKLM:\SOFTWARE\Microsoft\Virtual Machine\Guest\Parameters").GetValue("HostName")
```