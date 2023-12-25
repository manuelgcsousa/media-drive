## Basic 'exFAT' media drive setup (no fstab)

Create folder in which to mount drive:
```bash
$ sudo mkdir /mnt/<MOUNT_FOLDER>
```

Identify device name, get partition UUID and get user UID number:
```bash
$ lsblk
$ sudo blkid
$ grep ^"$USER" /etc/group
```

Mount drive using the device UUID:
```bash
$ sudo mount -o rw,users,uid=<USER_UID>,dmask=007,fmask=117 UUID=<DEVICE_UUID> /mnt/<MOUNT_FOLDER>
```

Unmount drive before ejecting:
```bash
$ sudo umount UUID=<DEVICE_UUID>
```

### Usage example:
```bash
#!/usr/bin/bash

USER_UID="..."
DEVICE_UUID="..."
MOUNT_FOLDER="/mnt/..."

MOUNT="sudo mount -o rw,users,uid=$USER_UID,dmask=007,fmask=117 UUID=$DEVICE_UUID $MOUNT_FOLDER"
UNMOUNT="sudo umount UUID=$DEVICE_UUID"

# mount drive and defer unmount
eval $MOUNT
trap "eval $UNMOUNT" INT

# start media server
# ...
```
