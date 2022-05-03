# AD Time Sync to NTP
#activedirectory #network #powershell 

Use Group policy configuration for PDC to sync with external time servers.

```powershell
NtpServer: us.pool.ntp.org,0x9 1.us.pool.ntp.org,0x9 2.us.pool.ntp.org,0x9 3.us.pool.ntp.org,0x9
Type: NTP
CrossSiteSyncFlags: 2
ResolvePeerBackoffMinutes: 15
ResolvePeerBackoffMaxTimes: 7
SpecialPollInterval: 3600
EventLogFlags: 0
```

GPO Filtering for the primary domain controller

```powershell
Select * from Win32_ComputerSystem where DomainRole = 5
```
> Enable NTP Client and Enable NTP Server should also be enabled

Force nonprimary domain controllers to sync with the PDC:

```powershell
w32tm /config /syncfromflags:domhier /update
```

If a domain controller is stuck and lists a previous PDC as its time server run:

```powershell
w32tm.exe /resync /rediscover
```

### Troubleshooting Virtualized domain controllers

Tell your virtualized Domain Controllers to ignore their Hyper-V host as a time source.

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider /v Enabled /t reg_dword /d 0
```

Tell the Virtual DC to sync its time with the PDC:

```powershell
w32tm /config /syncfromflags:DOMHIER /update
```

Restart the windows time service and force it to resync:

```powershell
net stop w32time & net start w32time
w32tm /resync /force
```

Force Resync:

```powershell
w32tm /query /source
```

Reset and reregister windows time settings:

```powershell
Net Stop W32time
W32tm.exe /unregister
W32tm.exe /register
Net Start W32time
w32tm /config /syncfromflags:domhier /update
w32tm /query /source
```

### Good resources:
https://theitbros.com/configure-ntp-time-sync-group-policy/

https://serverfault.com/questions/486593/hyper-v-time-sync-for-vm-domain-controller

http://www.sysadminlab.net/windows/configuring-ntp-on-windows-using-gpo