# Query Active Directory for last logon times of users
#powershell #sysadmin #windows 

https://interworks.com/blog/trhymer/2014/01/22/powershell-get-last-logon-all-users-across-all-domain-controllers/

Here is a quick PowerShell script to help you query the last logon time for all of your users across all of your domain controllers. It will also save the output to a .csv file specified in the $exportFilePath string.

I was surprised not to find many examples of this across the web already. Either not many people have multiple DC’s necessitating this or they have another more “enterprise” way of achieving the same results. Or, I’m just missing something really obvious! Either way, it was a fun script to throw together, and I hope it saves you some time in your quest to clean up AD.

```powershell
Import-Module ActiveDirectory
 
function Get-ADUsersLastLogon()
{
  $dcs = Get-ADDomainController -Filter {Name -like "*"}
  $users = Get-ADUser -Filter *
  $time = 0
  $exportFilePath = "C:\lastLogon.csv"
  $columns = "name,username,datetime"

  Out-File -filepath $exportFilePath -force -InputObject $columns

  foreach($user in $users)
  {
    foreach($dc in $dcs)
    { 
      $hostname = $dc.HostName
      $currentUser = Get-ADUser $user.SamAccountName | Get-ADObject -Server $hostname -Properties lastLogon

      if($currentUser.LastLogon -gt $time) 
      {
        $time = $currentUser.LastLogon
      }
    }

    $dt = [DateTime]::FromFileTime($time)
    $row = $user.Name+","+$user.SamAccountName+","+$dt

    Out-File -filepath $exportFilePath -append -noclobber -InputObject $row

    $time = 0
  }
}
 
Get-ADUsersLastLogon
```