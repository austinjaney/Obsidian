# Veeam ports for Hyper-v
#veeam #sysadmin #windows #server #windows 

REM Rules are from here: http://www.veeam.com/kb1518  

REM On Hyper-V Host Server open these ports
```powershell
netsh advfirewall firewall add rule name="VEEAM Backup and Replication TCP" dir=in action=allow protocol=TCP localport=135,137-139,445  
netsh advfirewall firewall add rule name="VEEAM Backup and Replication UDP" dir=in action=allow protocol=UDP localport=135,137-139,445  
netsh advfirewall firewall add rule name="VEEAM Installer Service" dir=in action=allow protocol=TCP localport=6160  
netsh advfirewall firewall add rule name="VEEAM Backup Proxy Service" dir=in action=allow protocol=TCP localport=6162  
netsh advfirewall firewall add rule name="VEEAM vPower NFS Service" dir=in action=allow protocol=TCP localport=6161  
netsh advfirewall firewall add rule name="VEEAM vPower NFS Service" dir=in action=allow protocol=UDP localport=6161  
netsh advfirewall firewall add rule name="VEEAM Standard NFS Ports TCP" dir=in action=allow protocol=TCP localport=111,2049-2059,1058-1068  
netsh advfirewall firewall add rule name="VEEAM Standard NFS Ports UDP" dir=in action=allow protocol=UDP localport=111,2049-2059,1058-1068  
netsh advfirewall firewall add rule name="VEEAM Dynamic RPC Range" dir=in action=allow protocol=TCP localport=49152-65535  
netsh advfirewall firewall add rule name="VEEAM Proxy Appliance SSH" dir=in action=allow protocol=TCP localport=22
```

REM On VEEAM Server Open this  

```powershell
netsh advfirewall firewall add rule name="VEEAM Backup Server" dir=in action=allow protocol=TCP localport=2500-5000
```