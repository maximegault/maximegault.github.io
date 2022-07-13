---
title: Cron
date: 2021-05-10 14:00:00 HH:MM:SS +100
categories: [System, Linux]
tags: [system, linux, shell, tips, cron]
---

### List crontab

With the `-l` argument:

```bash
crontab -l
```

### Edit crontab

With the `-e` argument:

```bash
crontab -e
```

### Specify the user to interact with its crontab

With the `-u` argument:

```bash
crontab -u myUser
```

### Instructions

Right now just check [crontab guru](https://crontab.guru/)

### Delete crontab

With the `-r` argument:

```bash
crontab -r
```

### Allow/deny a user to use a crontab (on RHEL)

Just add the user name in one of these files:

* `/etc/cron.allow`
* `/etc/cron.deny`

### Backup crontab

```bash
crontab -l > myCrontabBackup
```

### Update crontab from file

It replaces the current crontab with the one in the input file

```bash
crontab myCrontabUpdateFile
```

### Example: add a line in `myUser` user's crontab from script

```bash
# Backup actual crontab
crontab -l -u myUser > crontab_myUser_backup
NEW_LINE='the line to be added'
echo "${NEW_LINE}" >> crontab_myUser_backup
crontab -u myUser crontab_myUser_backup
rm -f crontab_myUser_backup
```
