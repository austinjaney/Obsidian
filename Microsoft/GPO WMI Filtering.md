# Group Policy WMI Filtering
#sysadmin #activedirectory

WMI Filtering by Windows Build number, in this example 1909
https://support.microsoft.com/sw-ke/help/3119213/wmi-group-policy-filters-that-compare-win32-operatingsystem-buildnumbe

```
select * from Win32_OperatingSystem where Version like "10.0.18363" and ProductType="1"
```

Filter for Windows 10

```
select * from Win32_OperatingSystem where Version like "10.%" and ProductType="1"
```

Filter for Windows Desktops

```
select * from Win32_OperatingSystem WHERE ProductType = "1"
```

Filter for 62Bit Windows Servers

```
select * from Win32_OperatingSystem where (ProductType = "2") OR (ProductType = "3") AND  OSArchitecture = "64-bit"
```