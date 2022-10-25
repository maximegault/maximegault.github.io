---
title: Users and groups
date: 2022-05-13 09:00:00 HH:MM:SS +0100
categories: [System, Linux]
tags: [system, linux, shell, tips, users, groups]
---

### Groups

#### Create a group

Create a group with a given GID (thanks to `-g` argument) and a given name:

```bash
groupadd -g <gid> <groupName>
```

Example:

```bash
groupadd -g 1000 myGroup
```

#### Delete a group

With the `groupdel` command:

```bash
groupdel <groupName>
```

Example:

```bash
groupdel myGroup
```

#### List existing groups

```bash
cat /etc/group
```

Example output:

```text
input:x:105:
kvm:x:106:
render:x:107:
crontab:x:108:
netdev:x:109:
myGroup:x:1000:
ssh:x:110:
```

#### List current user groups

```bash
groups
```

Example output:

```text
myGroup secondaryGroup
```

### Users

#### Create a user

Create a user with:

* `-d`: its home directory
* `-u`: its UID
* `-g`: its GID (warning: `-g` belongs to the user's primary group)

Followed by its name:

```bash
useradd -d <homeDirectory> -u <uid> -g <gid> <userName>
```

Example:

```bash
useradd -d /home/myUser -u 1000 -g 1000 myUser
```

#### Delete a user

With the `userdel` command:

```bash
userdel <userName>
```

Example:

```bash
userdel myUser
```

Note: when a user is deleted, its primary group is also deleted, unless that group also contains other users.

#### List existing users

```bash
cat /etc/passwd
```

Example output:

```text
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
```

#### User's groups

Change user's primary group with `-g`:

```bash
usermod -g myNewGroup myUser
```

Add user to secondary groups with combination of `-a` and `-G`:

```bash
usermod -a -G secondaryGroup,otherSecondaryGroup myUser
```

### Mixed

List a user's groups and ids:

```bash
id
```

Example output:

```text
uid=1000(myUser) gid=1000(rhds) groups=1000(myGroup),2000(secondaryGroup)
```
