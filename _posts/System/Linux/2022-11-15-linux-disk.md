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
