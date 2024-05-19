### How to mount new disk

display list block divices.

```
$ sudo lsblk

NAME                      MAJ:MIN RM         SIZE RO TYPE MOUNTPOINT
sda                         8:0    0  53687091200  0 disk
├─sda1                      8:1    0      1048576  0 part
├─sda2                      8:2    0   1073741824  0 part /boot
└─sda3                      8:3    0  52610203648  0 part
  └─ubuntu--vg-ubuntu--lv 253:1    0  52609155072  0 lvm  /
sdb                         8:16   0 214748364800  0 disk
```

display partition

```
$ sudo fdisk -l

...

Disk /dev/sdb: 200 GiB, 214748364800 bytes, 419430400 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x261397a8

...
```

change partition table and add a new partition

```
$ sudo fdisk /dev/sdb

...
#
# add a new partition
#
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-419430399, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-419430399, default 419430399):

Created a new partition 1 of type 'Linux' and of size 200 GiB.

#
# save and exit
#
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

$ sudo fdisk -l

...
Device     Boot Start       End   Sectors  Size Id Type
/dev/sdb1        2048 419430399 419428352  200G 83 Linux
...
```

change partition type

```
$ sudo fdisk /dev/sdb

...
#
# change partition type
#
Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'.

#
# save and exit
#
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.


$ sudo fdisk -l

...
Device     Boot Start       End   Sectors  Size Id Type
/dev/sdb1        2048 419430399 419428352  200G 8e Linux LVM
...

```

create physiacl volume

```
$ sudo pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created.

$ sudo pvdisplay
...
"/dev/sdb1" is a new physical volume of "<200.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb1
  VG Name
  PV Size               <200.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               n6RpOX-4vms-c9Wo-7yZt-Vhh2-YNDV-16anqr
```

creact volume group  
`vgcreate <NAME> <PV NAME>`

```
$ sudo vgcreate data-vg /dev/sdb1
  Volume group "data-vg" successfully created

$ sudo vgdisplay
...
  --- Volume group ---
  VG Name               data-vg
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <200.00 GiB
  PE Size               4.00 MiB
  Total PE              51199
  Alloc PE / Size       0 / 0
  Free  PE / Size       51199 / <200.00 GiB
  VG UUID               Hq3ncP-sUy8-Pw4S-lyXm-DbQe-SONn-zfJghU
...

$ sudo pvdisplay
...
  --- Physical volume ---
  PV Name               /dev/sdb1
  VG Name               data-vg
  PV Size               <200.00 GiB / not usable 3.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              51199
  Free PE               51199
  Allocated PE          0
  PV UUID               n6RpOX-4vms-c9Wo-7yZt-Vhh2-YNDV-16anqr

...
```

create logical volume
`lvcreate -L <size> -n <lvname> <vgname>`

```
$ sudo lvcreate -L 199.9G -n data-lv data-vg
  Rounding up size to full physical extent 199.90 GiB
  Logical volume "data-lv" created.

$ sudo lvdisplay
...
  --- Logical volume ---
  LV Path                /dev/data-vg/data-lv
  LV Name                data-lv
  VG Name                data-vg
  LV UUID                LMN5Te-Dds3-DgMR-gT3Y-hJir-Xcun-i9Cr5z
  LV Write Access        read/write
  LV Creation host, time master, 2021-01-13 03:41:13 +0000
  LV Status              available
  # open                 0
  LV Size                199.90 GiB
  Current LE             51175
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
...

$ sudo fdisk -l
...
Disk /dev/mapper/data--vg-data--lv: 199.92 GiB, 214643507200 bytes, 419225600 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
...
```

create filesystem

```
$ sudo mkfs.ext4 /dev/mapper/data--vg-data--lv

mke2fs 1.45.5 (07-Jan-2020)
Discarding device blocks: done
Creating filesystem with 52403200 4k blocks and 13107200 inodes
Filesystem UUID: ff9ea057-c620-4750-9a71-215568abfef0
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424, 20480000, 23887872

Allocating group tables: done
Writing inode tables: done
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: done
```

auto mount config and reboot

```
$ sudo vi /etc/fstab
...
# add new line
/dev/mapper/data--vg-data--lv /linoxide ext4 defaults 1 2
...

$ sudo systemctl reboot
.
.
.

$ df -h
Filesystem                         Size  Used Avail Use% Mounted on
...
/dev/mapper/data--vg-data--lv      196G   61M  186G   1% /linoxide
...
```
