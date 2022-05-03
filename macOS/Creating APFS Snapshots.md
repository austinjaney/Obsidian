# Creating APFS Snapshots
#macos #support 

Taking a full disk snapshot:
```bash
Tmutil snapshot /
```

```bash
MLT-AJANEY:~ ajaney$ tmutil snapshot /
Created local snapshot with date: 2018-07-20-075913
```

You can list current snapshots by using:
```bash
tmutil listlocalsnapshots /
com.apple.TimeMachine.2019-02-26-133811
```

Restoring a snapshot can be done by rebooting into recovery mode and using time machine.