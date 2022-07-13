---
title: Ulimit
date: 2020-10-28 09:10:00 HH:MM:SS +0100
categories: [System, Linux]
tags: [system, linux, shell, ulimit]
---

`ulimit` is a command that enables to put limits on the amount of various system resources available to a user process (ie. max number of open files, the priority to run user process with, etc.).

If we need theses values to be persistent after reboots, just set the ulimit values in the `/etc/security/limits.conf` file or in a specific `file.conf` file in the `/etc/security/limits.d` directory.

### See all the current user limits

```bash
# H for hard limits, S for soft limits, a means all
$ ulimit -Sa
$ ulimit -Ha
```

Output example:

```text
core file size          (blocks, -c) unlimited
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 63264
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 262144
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) unlimited
cpu time               (seconds, -t) unlimited
max user processes              (-u) 63264
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

### See a specific value for the current user

```bash
ulimit -Su
```

Output example:

```text
63264
```

### See all the limits for another user

```bash
su - user -c "ulimit -a"
```
