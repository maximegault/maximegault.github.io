---
title: Disk commands
date: 2022-11-15 14:00:00 HH:MM:SS +0100
categories: [System, Linux]
tags: [system, linux, shell, tips, disk]
---

### List information about block devices

With `lsblk` (see [man](https://man7.org/linux/man-pages/man8/lsblk.8.html)), we can get the list and characteristics of disks and their partitions. Example output:

```text
NAME        MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
nvme0n1     259:1    0  50G  0 disk 
└─nvme0n1p1 259:2    0  50G  0 part /
nvme1n1     259:0    0  50G  0 disk
```

### Find a (mounted) filesystem

`findmnt` (see [man](https://man7.org/linux/man-pages/man8/findmnt.8.html)) display a list of all mounted file systems or search for a file system. Example output:

```text
TARGET                                SOURCE         FSTYPE      OPTIONS
/                                     /dev/nvme0n1p1 xfs         rw,relatime,seclabel,attr2,inode64,noquota
├─/sys                                sysfs          sysfs       rw,nosuid,nodev,noexec,relatime,seclabel
│ ├─/sys/kernel/security              securityfs     securityfs  rw,nosuid,nodev,noexec,relatime
│ ├─/sys/fs/cgroup                    tmpfs          tmpfs       ro,nosuid,nodev,noexec,seclabel,mode=755
│ │ ├─/sys/fs/cgroup/systemd          cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,xattr,release_agent=/usr/lib/systemd/systemd-cgroups-agent,name=systemd
│ │ ├─/sys/fs/cgroup/cpuset           cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,cpuset
│ │ ├─/sys/fs/cgroup/pids             cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,pids
│ │ ├─/sys/fs/cgroup/net_cls,net_prio cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,net_prio,net_cls
│ │ ├─/sys/fs/cgroup/freezer          cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,freezer
│ │ ├─/sys/fs/cgroup/cpu,cpuacct      cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,cpuacct,cpu
│ │ ├─/sys/fs/cgroup/perf_event       cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,perf_event
│ │ ├─/sys/fs/cgroup/memory           cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,memory
│ │ ├─/sys/fs/cgroup/hugetlb          cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,hugetlb
│ │ ├─/sys/fs/cgroup/blkio            cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,blkio
│ │ └─/sys/fs/cgroup/devices          cgroup         cgroup      rw,nosuid,nodev,noexec,relatime,seclabel,devices
│ ├─/sys/fs/pstore                    pstore         pstore      rw,nosuid,nodev,noexec,relatime
│ ├─/sys/fs/selinux                   selinuxfs      selinuxfs   rw,relatime
│ ├─/sys/kernel/config                configfs       configfs    rw,relatime
│ └─/sys/kernel/debug                 debugfs        debugfs     rw,relatime
├─/proc                               proc           proc        rw,nosuid,nodev,noexec,relatime
│ └─/proc/sys/fs/binfmt_misc          systemd-1      autofs      rw,relatime,fd=28,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=998
│   └─/proc/sys/fs/binfmt_misc        binfmt_misc    binfmt_misc rw,relatime
├─/dev                                devtmpfs       devtmpfs    rw,nosuid,seclabel,size=7957576k,nr_inodes=1989394,mode=755
│ ├─/dev/shm                          tmpfs          tmpfs       rw,nosuid,nodev,seclabel
│ ├─/dev/pts                          devpts         devpts      rw,nosuid,noexec,relatime,seclabel,gid=5,mode=620,ptmxmode=000
│ ├─/dev/mqueue                       mqueue         mqueue      rw,relatime,seclabel
│ └─/dev/hugepages                    hugetlbfs      hugetlbfs   rw,relatime,seclabel
├─/run                                tmpfs          tmpfs       rw,nosuid,nodev,seclabel,mode=755
│ └─/run/user/1000                    tmpfs          tmpfs       rw,nosuid,nodev,relatime,seclabel,size=1596316k,mode=700,uid=1000,gid=1000
└─/var/lib/nfs/rpc_pipefs             rpc_pipefs     rpc_pipefs  rw,relatime
```

### Manipulate disk partition table

`fdisk` (see [man](https://man7.org/linux/man-pages/man8/fdisk.8.html)) allows to create, delete, list partitions on a hard disk.

> `fdisk` needs administrator rights.
{: .prompt-warning }

#### List the partition tables for the specified devices

With the `-l` option only, it will list all the partitions from all the devices:

```bash
sudo fdisk -l
```

Example output:

```text
Disk /dev/nvme1n1: 53.7 GB, 53687091200 bytes, 104857600 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/nvme0n1: 53.7 GB, 53687091200 bytes, 104857600 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000b0d11

        Device Boot      Start         End      Blocks   Id  System
/dev/nvme0n1p1   *        2048   104857566    52427759+  83  Linu
```

Specifying a device name, it will list only the partitions related to that device:

```bash
sudo fdisk -l /dev/nvme1n1
```

Example output:

```text
Disk /dev/nvme1n1: 53.7 GB, 53687091200 bytes, 104857600 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

### Full example

#### Creation

##### List disks with `sudo fdisk -l`

```text
Disk /dev/sda: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes

Disk /dev/sdb: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes

Disk /dev/sdc: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x94a4803e

Disk /dev/sdd: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x032632cb

Disk /dev/sde: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes

Disk /dev/sdf: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
```

##### Check existing partitions with `lsblk`

```text
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  256G  0 disk
sdb      8:16   0  256G  0 disk /
sdc      8:32   0  256G  0 disk
sdd      8:48   0  256G  0 disk
sde      8:64   0  256G  0 disk
sdf      8:80   0  256G  0 disk
```

##### Create a partition on `/dev/sdd` with the `sudo fdisk /dev/sdd` command

This is an interactive prompt:

* `d`: delete a partition
* `n`: add a new partition
* `p`: print the partition table
* `w`: write table to disk and exit

So create a partition with `n`, type the needed parameters and validate with `w`.

##### Check existing partitions with `lsblk` again

```text
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  256G  0 disk
sdb      8:16   0  256G  0 disk /
sdc      8:32   0  256G  0 disk
sdd      8:48   0  256G  0 disk
└─sdd1   8:49   0  256G  0 part
sde      8:64   0  256G  0 disk
sdf      8:80   0  256G  0 disk
```

##### List disks with `sudo fdisk -l` again

```text
Disk /dev/sda: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes

Disk /dev/sdb: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes

Disk /dev/sdc: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x94a4803e

Disk /dev/sdd: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x032632cb

Device     Boot Start       End   Sectors  Size Id Type
/dev/sdd1        2048 536870911 536868864  256G 83 Linux

Disk /dev/sde: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes

Disk /dev/sdf: 256 GiB, 274877906944 bytes, 536870912 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
```

##### Format the partition with `mkfs` command

Here we have created the `/dev/sdd1` partition, so we can use `sudo mkfs /dev/sdd1` (default partition type can be `ext2`) or specify a partition type (like `xfs`) with the command `sudo mkfs.xfs /dev/sdd1`.

> `file` command can help to know an already formatted partition type, ie. `sudo file -sL /dev/sdc1`.
{: .prompt-tip }

##### Create a directory to mount the partition on

Example `sudo mkdir -p /myDirectory/subDirectory`.

##### Mount the partition

Example `sudo mount /dev/sdd1 /myDirectory/subDirectory`.

##### Check the mount point with `findmnt`

```text
TARGET                          SOURCE       FSTYPE      OPTIONS
/                               /dev/sdf     ext4        rw,relatime,discard,errors=remount-ro,data=ordered
├─/mnt/wsl                      tmpfs        tmpfs       rw,relatime
├─/init                         tools[/init] 9p          ro,relatime,dirsync,aname=tools;fmask=022,loose,access=client,trans=fd,rfd=6,wfd=6
├─/dev                          none         devtmpfs    rw,nosuid,relatime,size=13073648k,nr_inodes=3268412,mode=755
│ └─/dev/pts                    devpts       devpts      rw,nosuid,noexec,noatime,gid=5,mode=620,ptmxmode=000
├─/sys                          sysfs        sysfs       rw,nosuid,nodev,noexec,noatime
│ └─/sys/fs/cgroup              tmpfs        tmpfs       rw,nosuid,nodev,noexec,relatime,mode=755
│   ├─/sys/fs/cgroup/unified    cgroup2      cgroup2     rw,nosuid,nodev,noexec,relatime,nsdelegate
│   ├─/sys/fs/cgroup/cpuset     cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,cpuset
│   ├─/sys/fs/cgroup/cpu        cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,cpu
│   ├─/sys/fs/cgroup/cpuacct    cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,cpuacct
│   ├─/sys/fs/cgroup/blkio      cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,blkio
│   ├─/sys/fs/cgroup/memory     cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,memory
│   ├─/sys/fs/cgroup/devices    cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,devices
│   ├─/sys/fs/cgroup/freezer    cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,freezer
│   ├─/sys/fs/cgroup/net_cls    cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,net_cls
│   ├─/sys/fs/cgroup/perf_event cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,perf_event
│   ├─/sys/fs/cgroup/net_prio   cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,net_prio
│   ├─/sys/fs/cgroup/hugetlb    cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,hugetlb
│   ├─/sys/fs/cgroup/pids       cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,pids
│   └─/sys/fs/cgroup/rdma       cgroup       cgroup      rw,nosuid,nodev,noexec,relatime,rdma
├─/proc                         proc         proc        rw,nosuid,nodev,noexec,noatime
│ └─/proc/sys/fs/binfmt_misc    binfmt_misc  binfmt_misc rw,relatime
├─/run                          none         tmpfs       rw,nosuid,noexec,noatime,mode=755
│ ├─/run/lock                   none         tmpfs       rw,nosuid,nodev,noexec,noatime
│ ├─/run/shm                    none         tmpfs       rw,nosuid,nodev,noatime
│ └─/run/user                   none         tmpfs       rw,nosuid,nodev,noexec,noatime,mode=755
├─/usr/lib/wsl/drivers          drivers      9p          ro,nosuid,nodev,noatime,dirsync,aname=drivers;fmask=222;dmask=222,mmap,access=client,msize=65536,trans=fd,rfd=4,wfd=4
├─/usr/lib/wsl/lib              lib          9p          ro,nosuid,nodev,noatime,dirsync,aname=lib;fmask=222;dmask=222,mmap,access=client,msize=65536,trans=fd,rfd=4,wfd=4
├─/mnt/c                        C:\          9p          rw,noatime,dirsync,aname=drvfs;path=C:\;uid=0;gid=0;symlinkroot=/mnt/,mmap,access=client,msize=65536,trans=fd,rfd=8,wfd=8
├─/mnt/g                        G:\          9p          rw,noatime,dirsync,aname=drvfs;path=G:\;uid=0;gid=0;symlinkroot=/mnt/,mmap,access=client,msize=65536,trans=fd,rfd=8,wfd=8
└─/myDirectory/subDirectory     /dev/sdd1    ext2        rw,relatime,stripe=4
```

#### Deletion

##### Unmount the partitition with `umount`

```bash
sudo umount /dev/sdd1
```

#### Delete the directory it's mounted on

```bash
sudo -Rf /myDirectory/subDirectory
```

### Delete the partition with `fdisk`

Delete the partition with `d` and validate with `w`.

Every step can be checked with the previous given commands.
