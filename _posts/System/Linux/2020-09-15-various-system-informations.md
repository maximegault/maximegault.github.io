---
title: Various system informations
date: 2020-09-15 10:30:00 HH:MM:SS +0200
categories: [System, Linux]
tags: [system, linux, shell, version, python]
---

### Know the distribution name and version

A way:

```bash
cat /etc/*-release
```

Output example:

```text
NAME="Red Hat Enterprise Linux"
VERSION="8.2 (Ootpa)"
ID="rhel"
ID_LIKE="fedora"
VERSION_ID="8.2"
PLATFORM_ID="platform:el8"
PRETTY_NAME="Red Hat Enterprise Linux 8.2 (Ootpa)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:redhat:enterprise_linux:8.2:GA"
HOME_URL="https://www.redhat.com/"
BUG_REPORT_URL="https://bugzilla.redhat.com/"
```

Another way:

```bash
hostnamectl
```

Output example:

```text
   Static hostname: myHostName
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 84044124ac3b46cb83b39110745763a9
           Boot ID: 65460d4645724ecabf3be02873edf0f0
    Virtualization: vmware
  Operating System: Red Hat Enterprise Linux 8.2 (Ootpa)
       CPE OS Name: cpe:/o:redhat:enterprise_linux:8.2:GA
            Kernel: Linux 4.18.0-193.el8.x86_64
      Architecture: x86-64
```

### Know the installed Python version

A way:

```bash
python --version
```

Output example:

```text
python 2.0.0
```

Another way:

```bash
python -V
```

Output example:

```text
python 3.0.0
```
