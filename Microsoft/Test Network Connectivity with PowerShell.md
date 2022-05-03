# Test Network Connectivity with PowerShell
#support #powershell #windows 

You can use PowerShell to test network connectivity instead of telnet:

```powershell
Test-NetConnection -ComputerName 192.168.0.1 -port 3610
```

> you can substitute Test-NetConnection with "tnc"