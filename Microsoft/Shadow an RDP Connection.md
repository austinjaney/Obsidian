# Shadow an RDP Connection
#sysadmin #support #powershell 

To shadow or ghost another user use the mstsc utility. 

```powershell
mstsc /shadow:ID /control
```

Example shadowing weston who is user ID5:

```powershell
PS C:\Windows\system32> quser
 USERNAME              SESSIONNAME        ID  STATE   IDLE TIME  LOGON TIME
>adm-ajaney            rdp-tcp#4           4  Active          .  1/30/2020 4:27 PM
 adm-wjones            rdp-tcp#6           5  Active          3  1/30/2020 4:28 PM
PS C:\Windows\system32> mstsc /shadow:5 /control
```