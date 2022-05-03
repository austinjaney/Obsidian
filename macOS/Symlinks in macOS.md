# Dropbox Symlink setup for macOS
#macos #linux 

Symlinks on Mac are not officially recommended or supported by Dropbox but they do seem fairly unbreakable regardless.
Symlink-ing Desktop and Documents

First cd to your dropbox folder

```bash
cd /Users/username/Dropbox
```


Then create symlinks

```bash
Ln -s ~/Desktop
Ln -s ~/Documents
```


If you already have another Mac syncing to dropbox using symlinks it is recommended to unlink that Mac before linking the new one.  If you need to sync both Macs document / desktop data you will want to disconnect the new computer from the internet before starting the sync process and stop dropbox before creating the symlinks.

https://www.imore.com/how-sync-your-documents-desktop-and-any-other-folder-dropbox