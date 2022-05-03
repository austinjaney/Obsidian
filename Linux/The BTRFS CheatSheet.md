# The BTRFS CheatSheet
#sysadmin #linux 

http://blog.programster.org/btrfs-cheatsheet
BTRFS (sometimes pronounced butter-fs) is a filesystem developed by Oracle (maintainers of Virtualbox and Java). It is a copy-on-write (CoW) filesystem, which allows one to implement advanced features such as snapshotting and bitrot detection/ removal.

The best reason to use BTRFS is that it allows one to add additional drives of any capacity to the array to increase overall storage capacity and performance, without wasting any individual drive's space. BTRFS is so flexible, that it has the ability to change RAID levels without disassembling the array. Both of these actions can be performed whilst the array is being used (no downtime).

## Checking Status
The following command should show if a disk has been kicked out of the array. It's mainly there to show you what filesystems you have and what disks are part of them.
btrfs fi show
Example output:

```
Label: none  uuid: 58bd01a7-f160-4fea-aed3-c378c2332699
        Total devices 6 FS bytes used 7.36TiB
        devid    1 size 2.73TiB used 2.61TiB path /dev/sda
        devid    2 size 2.73TiB used 2.61TiB path /dev/sdd
        devid    3 size 3.64TiB used 2.61TiB path /dev/sdf
        devid    4 size 2.73TiB used 2.61TiB path /dev/sdb
        devid    5 size 2.73TiB used 2.61TiB path /dev/sdc1
        devid    6 size 3.64TiB used 2.61TiB path /dev/sde
```

The following command will give you stats of any errors that have occurred.

```
btrfs device stats [path to filesystem or disk]
Example output:
[/dev/sda].write_io_errs   223539
[/dev/sda].read_io_errs    0
[/dev/sda].flush_io_errs   79
[/dev/sda].corruption_errs 0
[/dev/sda].generation_errs 0
[/dev/sdd].write_io_errs   0
[/dev/sdd].read_io_errs    0
[/dev/sdd].flush_io_errs   0
[/dev/sdd].corruption_errs 0
[/dev/sdd].generation_errs 0
[/dev/sdf].write_io_errs   0
[/dev/sdf].read_io_errs    0
[/dev/sdf].flush_io_errs   0
[/dev/sdf].corruption_errs 0
[/dev/sdf].generation_errs 0
[/dev/sdb].write_io_errs   0
[/dev/sdb].read_io_errs    0
[/dev/sdb].flush_io_errs   0
[/dev/sdb].corruption_errs 0
[/dev/sdb].generation_errs 0
[/dev/sdc1].write_io_errs   0
[/dev/sdc1].read_io_errs    0
[/dev/sdc1].flush_io_errs   0
[/dev/sdc1].corruption_errs 0
[/dev/sdc1].generation_errs 0
[/dev/sde].write_io_errs   0
[/dev/sde].read_io_errs    0
[/dev/sde].flush_io_errs   0
[/dev/sde].corruption_errs 0
[/dev/sde].generation_errs 0
```


## Filesystem
Filesystem Capacity
Show space usage information for a mount point.

```
btrfs filesystem df $PATH
```

Show BTRFS Filesystems
Show the info of a btrfs filesystem. If no path is passed, info of all the btrfs filesystems are shown.

```
btrfs filesystem show

```

Resize Filesystem
Resize the file system. If 'max' is passed, the filesystem will occupy all available space on the device.

```
btrfs filesystem resize $DEVICE_ID [+/-] $NEW_SIZE $FILESYSTEM
```

Grow a filesystem to fill a device.

```
btrfs filesystem resize $DEVICE_ID max $FILESYSTEM
```

## Balance Filesystem
Balancing a filesystem spreads the chunks of data across disks in an even manner. If disk's are unequal in capacity, eqaulity will be by percentage capacity utilized. E.g. all disks will be at the same % utilization. One should do this after adding more disks.

```
btrfs balance start [btrfs mount point]
```

A balance operation can take a very long time. Check the progress with:

```
btrfs balance status /path/to/mount
```

## Changing RAID Levels
Changing RAID levels is actually just an "advanced" balancing of the disks.

```
sudo btrfs balance start \  
-dconvert=[data raid level] \
-mconvert=[metadata raid level] \
[btrfs mount point]
```

Acceptable RAID levels are currently raid0, raid1, and raid10. Raid10 requires 4+ disks (can be an odd number of disks).

```
sudo btrfs balance start \  
-dconvert=raid10 \
-mconvert=raid10 \
/data
```


Defragment
Defragment a file or a directory.

```
btrfs filesystem defragment [btrfs filesystem path]

```

## Scrubbing
Start Scrub

Scrub filesystem to remove errors/bitrot

```
sudo btrfs scrub start /path/to/filesystem/mount
```

You can still use the filesystem whilst scrubbing is in progress. It takes place in the background with a low IO priority.

Check Scrub Status

```
sudo btrfs scrub status /path/to/filesystem/mount
```


## Devices
Add Device

```
btrfs device add -f /dev/sd[x] [btrfs mount point]
```

After adding a device, it's an extremely good idea to balance the filesystem.
Remove Device

```
btrfs device delete /dev/sd[x] [btrfs mount point]
```

Alternatively, if a device is missing:

```
btrfs device delete missing [btrfs mount point]
```

The array has to be on-line in order to delete a device. If your device has failed (and thus missing), then you need to mount the array in degraded mode before executing the command.
Mount Array In Degraded Mode

```
sudo mount -o degraded /dev/sd[x] [btrfs mount point]
```

I've only tested this where /dev/sd[x] is one of the working devices in the array, rather than the broken one.

## Sub-volumes And Snapshots
Subvolumes are the BTRFS equivalent of "datasets". E.g. they are areas of data that can be treated as a separate volume that can be snapshotted/restored etc. You may want to create a subvolume for each KVM guest etc.

Create Sub-volume

```
btrfs subvolume create
```

Show sub-volumes

```
btrfs subvolume list
```

Take Snapshot

```
btrfs subvolume snapshot
```

Delete Subvolume

```
btrfs subvolume delete
```


## References
|BTRFS Wiki |https://btrfs.wiki.kernel.org/index.php/Btrfs%28command%29 |
|BTRFS Resize|https://docs.oracle.com/cd/E37670_01/E37355/html/ol_use_case2_btrfs.html |
|Reddit - Status of drives in a btrfs RAID array? |https://www.reddit.com/r/btrfs/comments/3qo9px/status_of_drives_in_a_btrfs_raid_array/ |