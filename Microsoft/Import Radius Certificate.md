# Import Radius Certificate
#sysadmin #windows #powershell 

This script imports these three files
The jumpcloud cert is moved into the root certificate athority
The first wireless profile is moved in
The second wireless profile is moved in
 
Its worth noting that running this script twice will remove saved radius creditenals

```powershell
Import-Certificate -FilePath "C:\Windows\Temp\radius.jumpcloud.com.crt" -CertStoreLocation 'Cert:\LocalMachine\Root' -Verbose
netsh wlan add profile filename="C:\Windows\Temp\Wi-Fi-vizexplorer.xml" -verbose
netsh wlan add profile filename="C:\Windows\Temp\Wireless Network Connection-vizexplorer.xml"
```