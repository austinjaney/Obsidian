# Configure Powershell for Office 365
#powershell #office365

### Setup connection modules
```powershell
Install-Module MSOnline
Install-Module AzureAD
```

### Command to connect to microsoft online using modern auth
```powershell
Connect-MsolService
```