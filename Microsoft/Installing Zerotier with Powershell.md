# Installing Zerotier with Powershell
#sysadmin #powershell #windows 

### Install Zerotier with Powershell
 Zerotier uses a code signing certificate, due to the way that windows handles certificates and applications that require a trusted publisher its more difficult to install as it requires more steps 

 Getting the cert physically on the system (in our PDQ script we drop it in the public download folder) 

```powershell
Import-Certificate -Filepath "C:\Users\Public\Downloads\Zerotier\_TrustedPub.cer" -CertStoreLocation Cert:\LocalMachine\TrustedPublisher
```

 Installing zerotier using the .msi  

```powershell
cd "C:\Program Files (x86)\ZeroTier\One\" ; zerotier-cli.bat info 
cd "C:\Program Files (x86)\ZeroTier\One\" ; zerotier-cli.bat join 565799d8f63a49ff
```

spits out the current configuration info, since each zerotier client generates a unique ID on the fly when its installed this is useful if your identifying clients after deploying them in bulk 

then runs the command to join the client to the desired network

```powershell
Import-Certificate -Filepath "C:\Users\Public\Downloads\Zerotier_TrustedPub.cer" -CertStoreLocation Cert:\LocalMachine\TrustedPublisher
Import-Certificate [-FilePath] <String> [-CertStoreLocation <String>] [-WhatIf] [-Confirm] [<CommonParameters>]
```