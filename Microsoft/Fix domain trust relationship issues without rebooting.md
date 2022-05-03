# Fix domain trust relationship issues without rebooting
#sysadmin #support #cmd 

Apparently running the command

```powershell
Test-ComputerSecureChannel -Repair
```

Can fix domain trust relationship issues without issues.