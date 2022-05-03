# Connecting to Exchange Online using MFA
#sysadmin #office365 #exchange #powershell

Clipped from: <https://blogs.technet.microsoft.com/canitpro/2017/08/23/powershell-basics-connecting-to-exchange-online-using-multi-factor-authentication/>

Using PowerShell to manage your Microsoft cloud services like Exchange Online and using multi-factor authentication (MFA) separately is awesome. Using the two together however, not so much. Microsoft documentation on this topic seems to suggest that all the required administrative tasks needed are to be performed from a shell that launched separately from the PowerShell console. I would rather be able to connect to Exchange Online using MFA via PowerShell through a normal console, or as part of another tool. The following is how this can be achoplished

The first thing you'll need to do is [install the tool at this page](https://technet.microsoft.com/en-us/library/mt775114(v=exchg.160).aspx). This will give you all the tools and libraries you need to install to connect to Exchange Online using MFA via PowerShell, including that special, magic console. Now that you have the tools installed, you can use this snippet to connect from a normal PowerShell console or from within another PowerShell-based tool.

```powershell
$modules = @(Get-ChildItem -Path "$($env:LOCALAPPDATA)\Apps\2.0" -Filter "Microsoft.Exchange.Management.ExoPowershellModule.manifest" -Recurse )
$moduleName =  Join-Path $modules[0].Directory.FullName "Microsoft.Exchange.Management.ExoPowershellModule.dll"
Import-Module -FullyQualifiedName $moduleName -Force
$scriptName =  Join-Path $modules[0].Directory.FullName "CreateExoPSSession.ps1"
. $scriptName
$null = Connect-EXOPSSession
$exchangeOnlineSession = (Get-PSSession | Where-Object { ($_.ConfigurationName -eq 'Microsoft.Exchange') -and ($_.State -eq 'Opened') })[0]
```

Lines 1 and 2 detail the location of the different tools and libraries installed earlier. Once I find the ExoPowerShellModule.dll, I can import it like any other module, except I’m specifying the full path, on line 3.

Lines 4 and 5 are where I find and dot source CreateExoPSSession.ps1 which is the script that contains the Connect-EXOPSSession cmdlet (which I’d be remiss if I didn’t mention violates the PowerShell naming standards created by the community and advertised by Microsoft). That cmdlet will trigger a login process that includes MFA, similar to how Login-AzureRmAccount works.

Finally on lines 6 and 7, I’m creating a new session and then assigning it to a variable called $exchangeOnlineSession. Then I can import that session and I’ll be away to the races.

It’s not as convenient or straightforward as connecting without MFA, but it’s definitely safer.