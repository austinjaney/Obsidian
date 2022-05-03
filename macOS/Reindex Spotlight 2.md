# Reindex Spotlight
#macos 

On a mac we have had some issues where users complain that they cant search email in outlook, the fix for this seems to have to do with spotlight not working correctly or getting stuck.

to reindex the spotlight search on a mac run as root
```bash
sudo mdutil -E /
```

any drive can be indexed via targeting the volume
```bash
sudo mdutil -E /Volumes/MiniMe/
```