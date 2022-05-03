# Install Office 2019 with Group Policy
#activedirectory #powershell #windows 

This batch script is to install Office 2019 automaticaly via group policy to tirgger it at startup.  The script checks to see if Office 2019 is installed by checking the windows registery and if it is not attempts to exicute the install.

```bat
setlocal 
REM ********************************************************************* 
REM Environment customization begins here. Modify variables below. 
REM ********************************************************************* 
REM Set DeployServer to a network-accessible location containing the Office source files. 
set DeployServer=\\ithv2\Software\Office 2019\
REM Set ConfigFile to the configuration file to be used for deployment (required) 
set ConfigFile=\\ithv2\Software\Office 2019\config-Office2019-Standard-x64.xml
REM Set LogLocation to a central directory to collect script log files (install log files are set in XML file). 
set LogLocation=\\ithv2\Software\Office 2019\InstallLogs
REM ********************************************************************* 
REM Deployment code begins here. Do not modify anything below this line (check quotes are quotes though). 
REM ********************************************************************* 
IF NOT "%ProgramFiles(x86)%"=="" (goto ARP64) else (goto ARP86) 
REM Operating system is X64. Check for 32 bit Office in emulated Wow6432 registry key 
:ARP64 
reg query HKLM\SOFTWARE\WOW6432NODE\Microsoft\Office\15.0\ClickToRun\propertybag 
if NOT %errorlevel%==1 (goto End) 
REM Check for 32 and 64 bit versions of Office 2013 in regular registry key.(Office 64bit would also appear here on a 64bit OS) 
:ARP86 
reg query HKLM\SOFTWARE\Microsoft\Office\15.0\ClickToRun\propertybag 
if %errorlevel%==1 (goto DeployOffice) else (goto End) 
REM If 1 returned, the product was not found. Run setup here. 
:DeployOffice 
echo %date% %time% Setup started. >> %LogLocation%\%computername%.txt 
pushd "%DeployServer%"
start /wait setup.exe /configure "%ConfigFile%"
start C:\windows\system32\slmgr.vbs /ipk N698P-R2RJK-R4J6F-GWHFV-MBKRD
echo %date% %time% Setup ended with error code %errorlevel%. >> %LogLocation%\%computername%.txt 
REM If 0 or other was returned, the product was found or another error occurred. Do nothing. 
:End
Endlocal
```
