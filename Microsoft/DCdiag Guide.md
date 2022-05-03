# Dcdiag guide
#sysadmin #powershell #activedirectory

Dcdiag options: 

/s: dcname switch is used to run Dcdiag against a remote server 

/v: switch prints more detailed information about each test 

/c: switch means comprehensive, this will run all tests including the dns test. 

/q: switch will only print errors. This is useful as dcdiag can display a lot of information, if you want to see just the errors then use this switch. 

/f: switch is used to redirect the results to a file. 

TIP: When running dcdiag it will probably report some errors but this doesn’t necessarily mean you have issues with your domain controllers. For example, the command will query the system logs on the DC and display errors logs, but they could be errors from a computer or another server. Again this may not be a DC issue. You will just have to review and determine if it’s related or not. 

```powershell
dcdiag /c /e /v
```