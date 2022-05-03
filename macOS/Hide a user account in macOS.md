# Hide a user account in macOS
#macos #sysadmin 
https://support.apple.com/en-us/HT203998

If you need to assist a user, but don't want them to see your user account when they log in, learn how to hide a user account on the macOS login window.

This article is intended for system administrators. If you believe this issue affects you, contact the system administrator for your business or school.

Hide a user account in the macOS login window

Log in as an admin user.
Use this Terminal command. Substitute the short name of the user that you want to hide for "hiddenuser":

```bash
sudo dscl . create /Users/hiddenuser IsHidden 1
```

The user account is also hidden in System Preferences the next time it's opened. This command can't be used with the Guest user account. Learn how to manage the Guest user account.

!! Show a hidden user account

If you want to show the hidden user, set the user’s IsHidden attribute to 0:

```bash
sudo dscl . create /Users/hiddenuser IsHidden 0
```

If you want, you can delete the IsHidden attribute instead.
Hide the home directory and share point
You can move the hidden user's home directory to a place that's not visible from the Finder. And you can remove the hidden user's Public Folder share point.
This command moves the home directory of "hiddenuser" to /var, a hidden directory:

```bash
sudo mv /Users/hiddenuser /var/hiddenuser
```

This command updates the user record of "hiddenuser" with the new home directory path in /var:

```bash
sudo dscl . create /Users/hiddenuser NFSHomeDirectory /var/hiddenuser
```

This command removes the Public Folder share point for the user with the long name "Hidden User”:

```bash
sudo dscl . delete "/SharePoints/Hidden User's Public Folder"
```

Published Date: October 10, 2018