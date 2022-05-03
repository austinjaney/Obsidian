# Delete a folder with a trailing space
#windows #support #sysadmin #cmd

Experenced an issue today where Onedrive was attempting to sync a folder with a trailing space to a sharepoint site. 

The solution was to delete the "companyname.com" folder using the command:

```
rd /s "\\?\C:\Users\ajaney\northcoastchurch.com"
```

Another way to solve this is to directly delete the folder with a trailing space by targeting that folder directly. 

```
rd /s "\\?\C:\Users\ajaney\FolderWithTrailingSpace "
```