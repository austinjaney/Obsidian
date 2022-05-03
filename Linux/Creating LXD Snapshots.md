# Creating LXD Snapshots
#linux #ubuntu #sysadmin 

snapshot syntax

```bash
lxc snapshot {container} {snapshot-name}
```

Next, create the LXD snapshot:

```bash
lxc snapshot utls-newsletter snap-04-jan-2019
```

Verify snapsots or see info about snapshots:

```bash
lxc info utls-newsletter
```