# Query Azure AD for users MFA Phone Numbers
#office365 #powershell 

Find users MFA Phone Number

```powershell
(Get-MSOLUser -Userprincipalname <UPN>).StrongAuthenticationUserDetails.PhoneNumber
```
