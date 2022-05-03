# Automatic L2TP Configuration
#windows #powershell #northcoast

## Configure a VPN connection

> Configuring a VPN connection for us to reference in the reconnect.ps1 script.

```powershell
Add-VpnConnection -Name "North Coast Church" -ServerAddress "mail1.northcoastchurch.com" -TunnelType "L2tp" -EncryptionLevel "Required" -AuthenticationMethod MSChapv2 -UseWinlogonCredential -AllUserConnection -L2tpPsk "nccvpn"
```

### Reconnect.ps1
> This script is called by a scedualed task.

```powershell
# This script reconnects to the north coast VPN after a disconnect event is registered or 120 seconds and is triggered by a scedualed task.

# PreRequisits
## On the system that will be connecting to the vpn run the command to create the VPN connection on the computer.
### Add-VpnConnection -Name "North Coast Church" -ServerAddress "mail1.northcoastchurch.com" -TunnelType "L2tp" -EncryptionLevel "Required" -AuthenticationMethod MSChapv2 -UseWinlogonCredential -AllUserConnection -L2tpPsk "nccvpn"
## Both The Automatic Connect and Automatic Reconnect tasks must be run as the user attempting to connect.
### This script is referenced in both of those tasks.
## This scripts location must be in "c:\windows\system32\scripts\" so that the software restriction policy will allow it to run.
## The logged in user must be part of the "Vista VPN Users" Group in active directory so that they can authenticate via Radius.  The sonicwall is a Radius Client now.


$vpnName = "North Coast Church";
    $vpn = Get-VpnConnection -AllUserConnection -Name $vpnName;
    while($vpn.ConnectionStatus -eq "Disconnected"){
    rasdial $vpnName;
    Start-Sleep -Seconds 120
    }
```

### Reconnect.ps1 using username and password authentication with rasdial

```powershell
# This script reconnects to the north coast VPN after a disconnect event is registered or 120 seconds and is triggered by a scedualed task.

# PreRequisits
## On the system that will be connecting to the vpn run the command below to create the VPN connection on the computer.
### Add-VpnConnection -Name "North Coast Church" -ServerAddress "mail1.northcoastchurch.com" -TunnelType "L2tp" -EncryptionLevel "Required" -AuthenticationMethod MSChapv2 -RememberCredential -AllUserConnection -L2tpPsk "nccvpn"
## Both The Automatic Connect and Automatic Reconnect tasks must be run as the user attempting to connect after importing them.
### This script is referenced in both of those tasks.
## This scripts location must be in "c:\windows\system32\scripts\" so that the software restriction policy will allow it to run.
## The logged in user must be part of the "Vista VPN Users" Group in active directory or they must be local users with logon rights on the sonicwall so that they can authenticate.
## The username and password must be unique per NUC/Checkin Station

$vpnName = "North Coast Church";
    $vpn = Get-VpnConnection -AllUserConnection -Name $vpnName;
    while($vpn.ConnectionStatus -eq "Disconnected"){
    rasdial $vpnName rbcheckinnuc2 Arena1234;
    Start-Sleep -Seconds 120
    }
```