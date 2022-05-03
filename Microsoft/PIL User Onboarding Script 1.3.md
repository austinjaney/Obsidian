# PIL User Onboarding Script 1.3
#sysadmin #powershell #script

```powershell
### Partners in Leadership User onboarding script v1.3
### by Austin janey
### Purpose This script is designed to automate the onboarding process so that nothing slips threw the cracks during the onboarding process.
### In order for this script to work as intended it should be run on a domain controller user a domain admins account.
### Set-ExecutionPolicy Unrestricted may be needed in order for this script to run.

########################### What this script does ###########################
# we have a problem in that our onboarding process is not clean and takes forever to do via checklist
# this script automates a good chunk of what we do and gives us a default set of tickets that need to be completed in order for us to onboard a new employee
#
# This script as of version 1.3 does as follows
# Asks for the users first and last name and generates an active directory account based on that imput with a default password of Welcome123!
# Creates a folder named the new users ad login name in our home folder directory and sets the correct permissions so our group policy can add a shortcut to their desktop
# Asks if the new user needs an office 365 account, if so it sends a ticket requesting one to the helpdesk.
# Asks some questions about equipment that they will be getting and submits a ticket to setup their desk with a due date and details.
# asks if the new user needs a piltools account if so a ticket is sent to the piltools helpdesk requesting one
# asks if the new user needs a RingCentral account if so a ticket is sent to the IT helpdesk to create one.
# asks if the new user needs a Salesforce account if so a ticket is sent to the IT helpdesk to create one. 
# asks if the new user needs a Dropbox account if so a ticket is sent to the IT helpdesk to create one.
#
# gives a summary at the end of running the script of what was selected
# sends an email to the sysadmin team with the results of running the script.

########################### NOTES ###########################
### The Below line is the line used that will include the email information including the subject and body of the email.
### $PSEmailServer = "smtp.ozprinciple.net" ; Send-MailMessage -port 587 -To TOADDRESS@partnersinleadership.com -From "$mailsendfrom" -Subject “$Username requests a SOMETHING account” -Body “$Username is being onboarded! as a part of that $Username will need a SOMETHING account” -SmtpServer “smtp.ozprinciple.net” -Credential $creds
### in the Set Variable for Verbose username, used in setting up the user home folder acl permissions, this will need to be edited if we ever change domains
# $verboseadname="pil\$Username" -> to somedomain\username

########################### Change notes and feature request list ###########################
# [Complete] adds an active directory user to the default Users OU
# [Complete] creates a user home folder for the user and applies the correct permission set to the home folder location
# [Complete] asks if the user needs a salesforce account if yes is selected an email is sent to the ticket system requesting one
# [Complete] asks if the user needs a dropbox account if yes is selected an email is sent to the ticket system requesting one
# [Complete] asks for a piltools account by sending an email to contact@piltools.com and give the option for a note to be added.
# [Complete] asks if the new user needs a RingCentral account, if so a ticket is submitted to the helpdesk to set one up.
# [Complete] asks what kind of computer the user needs and generates a setup ticket
# [Complete] asks if the new user need an adobe creative cloud account.
# [Complete] Generates a report of what was done and sends it to the sysadmin team so we can make the welcome sheet

# [incomplete] asks what groups the user should be added to
# [incomplete] uses office 365 powershell to provision an office 365 user


########################### WELCOME SYSADMIN ###########################
echo "Welcome to the Partners in Leadership onboarding script"




########################### VARIABLE SETUP ###########################
### Active Directory Setup Variables
### Set Variable for your active directory domain
$addomain = "pil.ozprinciple.com"
### Set Variable for firstname, this will be concatinated with the users lastname to create their active directory name
$Firstname = Read-Host -Prompt 'please input the new users first name'
### Set Variable for lastname, explained above
$Lastname = Read-Host -Prompt 'please input the new users last name'
### Set the Variable for the users desired email address
$useremail = Read-Host -Prompt 'what is their desired email address?'
### Set Variable for fullname, this is used for the full name field for their active directory account
$Fullname = "$Firstname $Lastname"
### Set Variable for active directory username, creates their login username from their first and last names, this is also used to create their home folder
$Username = "$Firstname.$Lastname"

### Variables for setting up the user home folder
### Set Variable for Verbose username, used in setting up the user home folder acl permissions,
#!! this will need to be edited if we ever change domains
$verboseadname="pil\$Username"
### Set Path of the new Home Folder
$Path="\\pil.ozprinciple.com\Data\U\$Username"
### Set the access level for the user
$Access = "Modify"
### Force, included in the origional script, not sure if nesessary "Applies the ACL to an existing folder instead of throwing an error."
$Force
### Set Variables for automated emails so we can generate tickets
$onboardemail = “$mailsendfrom”
$PWord = ConvertTo-SecureString –String “YJ6eKRkVR!2p1o” –AsPlainText -Force
$Creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList '$onboardemail', $pword

### Variables for email
### $reportsendto should be the sysadmin DG or whoever might need to know what the answers to running your script were.
$reportsendto = "sysadmins@ozprinciple.com"
### the send to address for the email, this should be your helpdesk email address
$mailsendto = "pilhelp@ozprinciple.com"
### $mailsendfrom is the email address you are using to send the email from, this should be an account only used for submiting tickets in an automated fasion
$mailsendfrom = "onboard@ozprinciple.net"
$onboardemail = "onboard@ozprinciple.net"
$PWord = ConvertTo-SecureString –String “YJ6eKRkVR!2p1o” –AsPlainText -Force
$Creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $onboardemail, $pword

### Variables for date in ticket subject of office 365 setup
$emailsetupdate = Get-Date



########################### ACTIVE DIRECTORY SETUP USER ###########################
### Create an Active Directory User
### Set password Variable for first time login of new user
$adpassword = "Welcome123!" | ConvertTo-SecureString -AsPlainText -Force

### Set UserPrincipalName Variable
$UserPrincipalName = $Username+"@"+$addomain

### Adding the new user
New-ADUser -Name $Username -GivenName $Firstname -Surname $Lastname -Path "CN=Users,DC=pil,DC=ozprinciple,DC=Com" -homedirectory "\\pil.ozprinciple.com\data\U\$Username" -AccountPassword $adpassword -DisplayName  $Fullname -UserPrincipalName $UserPrincipalName -ChangePasswordAtLogon $True -Enabled $True




########################### ACTIVE DIRECTORY SETUP HOME DIRECTORY ###########################
### Create User Home Folder on \\pil.ozprinciple.com\Data\U\
# Build Access Rule for ACL:
$AccessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($verboseadname,$Access,"ContainerInherit, ObjectInherit", "None", "Allow")

# Create User Directory
New-Item -Path $Path -ItemType Directory

# Combine existing folder ACL with explicit permissions from the new access rule. 
$HomeDirectoryACL = Get-Acl -Path $Path
$HomeDirectoryACL.SetAccessRule($AccessRule)

# Apply ACL to destination folder
Set-Acl -Path $Path -AclObject $HomeDirectoryACL
Get-ACL -Path $Path | Select-Object Path, AccessToString | Format-List


clear


########################### BEGIN EMAIL TICKET CREATION QUESTIONS ###########################

### Asks/generate a reminder for the Sysadmin to create an office 365 account if the user needs a Office 365 account.
$title = "Office 365 Setup"
$message = "Does $Fullname need a Office 365 account? If yes is selected a ticket will be generated to remind the sysadmin team to create one."

$createdoffice365 = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", `
    "Sends a ticket to the PIL help desk requesting a Office 365 account for $Fullname."

$dontcreatedoffice365 = New-Object System.Management.Automation.Host.ChoiceDescription "&No", `
    "this user does not require a Office 365 account."

$options = [System.Management.Automation.Host.ChoiceDescription[]]($createdoffice365, $dontcreatedoffice365)

$result = $host.ui.PromptForChoice($title, $message, $options, 0)

switch ($result)
    {
        0 {"A ticket reminder to setup an office 365 account for $Fullname has been submitted";$PSEmailServer = "smtp.ozprinciple.net" ; Send-MailMessage -port 587 -To "$mailsendto" -From "$mailsendfrom" -Subject “$Fullname needs an Office 365 account Due $emailsetupdate” -Body “$Fullname is joining the team! $Fullname needs an office 365 account their desired email address is $useremail, Dont forget to add them to the correct distribution groups, add their manager and add their job title in mail settings.” -SmtpServer “smtp.ozprinciple.net” -Credential $creds ; $emailanswer = "Submitted a ticket to create an office 365 account with the email address $useremail"}
        1 {"$Fullname does not need a Office 365 account no ticket for this was generated" ; $emailanswer = "Chosen not to create $fullname a office 365 email address."}
    }


clear


### Asks the Sysadmin if the user will be working in office or remote, if they are working in office a ticket will be generated to do their desk setup.
### Asks what they need for their office / homeoffice
echo "Lets answer some questions about their desk setup"

$computertype = Read-Host -Prompt 'What type of computer will they be given? For (example: macbook air)'
$monitors = Read-Host -Prompt 'Will they be getting any monitors? if so how many? (example: 2 apple cinima displays)'
$invintorynotes = Read-Host -Prompt 'What are the asset tags for the computer and monitors?'
$cubiclenumber = Read-Host -Prompt 'where will they be sitting? (example: across from cidny in suite 400)'
$setupdate = Read-Host -Prompt 'When are they Joining the team? (example: 10/5/2020)'
$othernotes = Read-Host -Prompt 'Anything else to add as a note? (example: Note: we need to order a headset.)'

$title = "Desk Setup"
$message = "Will This user be working in office?, if inoffice is selected a ticket will be generated to setup their desk, if remote is selected a ticket will be generated for their equipment."

$inoffice = New-Object System.Management.Automation.Host.ChoiceDescription "&inoffice", `
    "Submits a ticket to the PIL help desk to setup a desk in office for $Fullname."

$remote = New-Object System.Management.Automation.Host.ChoiceDescription "&remote", `
    "Submits a ticket to the PIL help desk with what $Fullname needs for their remote office."

$options = [System.Management.Automation.Host.ChoiceDescription[]]($inoffice, $remote)

$result = $host.ui.PromptForChoice($title, $message, $options, 0)

switch ($result)
    {
        0 {"A ticket will be generated for $Fullname to have their desk and computer equipment setup";$PSEmailServer = "smtp.ozprinciple.net" ; Send-MailMessage -port 587 -To "$mailsendto" -From "$mailsendfrom" -Subject “$Fullname Desk Setup due $setupdate” -Body “$Fullname is joining the team on $setupdate! As a part of the onboarding it was requested that $Fullname be given a $computertype with $monitors (monitors). $fullname will be sitting $cubiclenumber. invintory notes for this ticket $invintorynotes. $othernotes” -SmtpServer “smtp.ozprinciple.net” -Credential $creds ; $desksetupanswer = "Submitted a ticket to setup a workspace $fullname with details about what computer they will be getting"}
        1 {"A ticket will be generated for $Fullname to have their computer setup";$PSEmailServer = "smtp.ozprinciple.net" ; Send-MailMessage -port 587 -To "$mailsendto" -From "$mailsendfrom" -Subject “$Fullname Equipment Deployment due $setupdate” -Body “$Fullname is joining the team on $setupdate! $Fullname will be working remote, As a part of the onboarding it was requested that $Fullname be given a $computertype with $monitors (monitors). invintory notes for this ticket $invintorynotes. $othernotes” -SmtpServer “smtp.ozprinciple.net” -Credential $creds ; $desksetupanswer = "Submitted a ticket to setup a computer $fullname to work remote with."}
    }


clear


### Asks the Sysadmin if the user needs a PILtools account
$title = "PILtools Setup"
$message = "Does $Fullname need a piltools account? If yes is selected a ticket will be generated to create one."

$createpiltools = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", `
    "Sends a ticket to the contact@piltools.com requesting an account for $Fullname."

$dontcreatepiltools = New-Object System.Management.Automation.Host.ChoiceDescription "&No", `
    "this user does not need a dropbox account."

$options = [System.Management.Automation.Host.ChoiceDescription[]]($createpiltools, $dontcreatepiltools)

$result = $host.ui.PromptForChoice($title, $message, $options, 0)

switch ($result)
    {
        0 {"A ticket will be generated for $Fullname to be provisioned a PILtools account";$PSEmailServer = "smtp.ozprinciple.net" ; Send-MailMessage -port 587 -To contact@piltools.com -From "$mailsendfrom" -Subject “$Fullname is joining the PIL team on $setupdate” -Body “$Fullname is being onboarded! As a part of the onboarding $Fullname will need a PILtools account make sure to check that an an email address has been made before completeing this onboarding request. Their email address will be $useremail.” -SmtpServer “smtp.ozprinciple.net” -Credential $creds ; $piltoolsanswer = "Submitted a ticket to PILtools to create $fullname a piltools account."}
        1 {"$username does not need a PILtools account." ; $piltoolsanswer = "Chosen not to submit a ticket to create $fullname a piltools account."}
    }


clear


### Asks the Sysadmin if the user needs a RingCentral account
$title = "RingCentral Setup"
$message = "Does $Fullname need a RingCentral account, if yes is selected a ticket will be generated to create one."

$setuprc = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", `
    "Sends a ticket to the it helpdesk requesting a RingCentral account for $Fullname."

$dontsetuprc = New-Object System.Management.Automation.Host.ChoiceDescription "&No", `
    "this user does not need a RingCentral account."

$options = [System.Management.Automation.Host.ChoiceDescription[]]($setuprc, $dontsetuprc)

$result = $host.ui.PromptForChoice($title, $message, $options, 0)

switch ($result)
    {
        0 {"A ticket will be generated for $Fullname to be provisioned a RingCentral account";$PSEmailServer = "smtp.ozprinciple.net" ; Send-MailMessage -port 587 -To "$mailsendto" -From "$mailsendfrom" -Subject “$Fullname is joining the PIL team on $setupdate” -Body “$Fullname is being onboarded! As a part of the onboarding $Fullname will need a RingCentral account make sure to check that an an email address has been made before completeing this onboarding request we expect their email address to be $useremail.” -SmtpServer “smtp.ozprinciple.net” -Credential $creds ; $ringcentralanswer = "Submitted a ticket to create $fullname a RingCentral account."}
        1 {"$username does not need a RingCentral account." ; $ringcentralanswer = "Chosen not to submit a ticket for $fullname to be given a RingCentral account."}
    }


clear


### Asks the Sysadmin if the user needs a Salesforce account
$title = "Salesforce Setup"
$message = "Does This user need a salesforce account, if yes is selected a ticket will be generated to create one."

$createsalesforce = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", `
    "Sends a ticket to the PIL help desk requesting a salesforce account for $Fullname."

$dontcreatesalesforce = New-Object System.Management.Automation.Host.ChoiceDescription "&No", `
    "this user does not need a salesforce account."

$options = [System.Management.Automation.Host.ChoiceDescription[]]($createsalesforce, $dontcreatesalesforce)

$result = $host.ui.PromptForChoice($title, $message, $options, 0)

switch ($result)
    {
        0 {"A ticket will be generated for $username to be provisioned a salesforce account.";$PSEmailServer = "smtp.ozprinciple.net" ; Send-MailMessage -port 587 -To "$mailsendto" -From "$mailsendfrom" -Subject “$Fullname is joining the team and needs a salesforce account” -Body “$Fullname is joining the team and will need a Salesforce account. Before Creating their Account please check with your sysadmins to make sure that we have created $Fullname a email address their email address should be $useremail.” -SmtpServer “smtp.ozprinciple.net” -Credential $creds ; $salesforceanswer = "Submitted a ticket to create $fullname a Salesforce account."}
        1 {"$username does not need a salesforce account." ; $salesforceanswer = "Chosen not to create a ticket for $fullname to be provided access to salesforce."}
    }


clear


### Asks the Sysadmin if the user needs a Dropbox account
$title = "Dropbox Setup"
$message = "Does This $Fullname need a dropbox account? If yes is selected a ticket will be generated to create one."

$createdropbox = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", `
    "Sends a ticket to the PIL help desk requesting a dropbox account for $Fullname."

$dontcreatedropbox = New-Object System.Management.Automation.Host.ChoiceDescription "&No", `
    "this user does not need a dropbox account."

$options = [System.Management.Automation.Host.ChoiceDescription[]]($createdropbox, $dontcreatedropbox)

$result = $host.ui.PromptForChoice($title, $message, $options, 0)

switch ($result)
    {
        0 {"A ticket will be generated for $Fullname to be provisioned a dropbox account";$PSEmailServer = "smtp.ozprinciple.net" ; Send-MailMessage -port 587 -To "$mailsendto" -From "$mailsendfrom" -Subject “$Fullname requests a dropbox account” -Body “$Fullname is being onboarded! As a part of the onboarding it was requested that $Fullname be added to our company dropbox” -SmtpServer “smtp.ozprinciple.net” -Credential $creds ; $dropboxanswer = "Submitted a ticket for $fullname to be provided access to dropbox."}
        1 {"$username does not need a dropbox account" ; $dropboxanswer = "Chosen not to provision $fullname a dropbox account."}
    }


clear


### Asks the Sysadmin if the user needs a Adobe Creative Cloud account
$title = "Adobe Creative Cloud Setup"
$message = "Does This $Fullname need a adobe creative cloud account? If yes is selected a ticket will be generated to create one."

$createacc = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", `
    "Sends a ticket to the PIL help desk requesting a adobe creative cloud account for $Fullname."

$dontcreateacc = New-Object System.Management.Automation.Host.ChoiceDescription "&No", `
    "this user does not need a adobe creative cloud account."

$options = [System.Management.Automation.Host.ChoiceDescription[]]($createdropbox, $dontcreatedropbox)

$result = $host.ui.PromptForChoice($title, $message, $options, 0)

switch ($result)
    {
        0 {"A ticket will be generated for $Fullname to be provisioned a adobe creative cloud account";$PSEmailServer = "smtp.ozprinciple.net" ; Send-MailMessage -port 587 -To "$mailsendto" -From "$mailsendfrom" -Subject “$Fullname requests an adobe creative cloud account” -Body “$Fullname is being onboarded! As a part of the onboarding it was requested that $Fullname be added to our company adobe creative cloud” -SmtpServer “smtp.ozprinciple.net” -Credential $creds ; $adobeccanswer = "Submitted a ticket to provide $fullname with an adobe creative cloud account."}
        1 {"$fullname does not need an acobe creative cloud account." ; $adobeccanswer = "chosen not to provide $fullname a adobe creative cloud account."}
    }


clear


########################### Answers ###########################
echo "perfect we have..."
# active directory answer
echo "Setup an active directory account for $username with the password Welcome123!"
# setup a home folder
echo "setup a home folder named $username at //pil.ozprinciple.com/data/u with the correct permissions"
# email setup answer
echo $emailanswer
# desk setup answer
echo $desksetupanswer
# piltools answer
echo $piltoolsanswer
# RingCentral answer
echo $ringcentralanswer
# Salesforce access answer
echo $salesforceanswer
# Dropbox answer
echo $dropboxanswer
# adobe creative cloud answer
echo $adobeccanswer

########################### Send a Report to Sysadmins ###########################
### sends a report to the sysadmin@ozprinciple.com DG for setup
$reportmessage = "$fullname is joining the team! $env:UserName ran the user onboarding script heres what happened. We created an active directory account and setup a home folder automaticaly, their user login is $username and their password is Welcome123!. We $emailanswer. We $desksetupanswer We $piltoolsanswer We $ringcentralanswer We $dropboxanswer Last but not least we $adobeccanswer"

### Asks the Sysadmin if we should send a report to the other sysadmins of this user onboarding
$title = "Onboarding Script Report email"
$message = "Should we send a report of what answers were selected using this onboarding script to the rest of the team?"

$sendreport = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", `
    "sends a report of what answers were selected during setup."

$dontsendreport = New-Object System.Management.Automation.Host.ChoiceDescription "&No", `
    "does not send a report."

$options = [System.Management.Automation.Host.ChoiceDescription[]]($sendreport, $dontsendreport)

$result = $host.ui.PromptForChoice($title, $message, $options, 0)

switch ($result)
    {
        0 {"A report has been sent to the sysadmin team about what answers were selceted during the onboarding script";$PSEmailServer = "smtp.ozprinciple.net" ; Send-MailMessage -port 587 -To "$reportsendto" -From "$mailsendfrom" -Subject “Onboarding Script report for $fullname” -Body “$reportmessage” -SmtpServer “smtp.ozprinciple.net” -Credential $creds}
        1 {"A report was not sent about this user onboarding to the sysadmin team"}
    }

########################### END of script ###########################
echo "Thanks for using the onboarding script! You can type exit to close the scripting window."
```