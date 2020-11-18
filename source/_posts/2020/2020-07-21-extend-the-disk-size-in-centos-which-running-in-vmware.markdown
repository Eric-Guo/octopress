---
layout: post
title: "Extend the disk size in CentOS which running in VMware"
date: 2020-07-21T15:25:24+08:00
comments: true
external-url:
categories: [CentOS]
---

Largely copy from [Linux Techi](https://www.linuxtechi.com/extend-lvm-partitions/)

## Check whether free space is available space in the volume group

```bash
[root@localhost dev]# vgdisplay
  --- Volume group ---
  VG Name               centos
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  9
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                3
  Open LV               3
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <199.00 GiB
  PE Size               4.00 MiB
  Total PE              50943
  Alloc PE / Size       28896 / <112.88 GiB
  Free  PE / Size       22047 / 86.12 GiB
  VG UUID               cUFFyw-wCaq-Ym07-TzQY-mii3-y8Xo-IdQRmn
```

## lvextend command to increase the size

```bash
[root@localhost ~]# lvextend -L +80G /dev/mapper/centos-root
  Size of logical volume centos/root changed from 100.00 GiB (25600 extents) to 180.00 GiB (46080 extents).
  Logical volume centos/root successfully resized.
```

## Run the resize2fs command

```bash
[root@localhost ~]# resize2fs /dev/mapper/centos-root
resize2fs 1.42.9 (28-Dec-2013)
resize2fs: Bad magic number in super-block 当尝试打开 /dev/mapper/centos-root 时
找不到有效的文件系统超级块.
```

## Resolve error due to root is xfs

```bash
[root@localhost ~]# cat /etc/fstab
# /etc/fstab
# Created by anaconda on Mon Dec 23 14:28:29 2019
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=48xxxx48-xxxx-xxxx-xxxx-xxxxxxxxxxxx /boot                   xfs     defaults        0 0
/dev/mapper/centos-home /home                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0

[root@localhost ~]# xfs_growfs /dev/mapper/centos-root
meta-data=/dev/mapper/centos-root isize=512    agcount=8, agsize=3276800 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=26214400, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=6400, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 26214400 to 47185920
```
