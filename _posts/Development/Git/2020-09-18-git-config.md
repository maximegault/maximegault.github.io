---
title: Git config
date: 2020-09-18 14:30:00 HH:MM:SS +0200
categories: [Development, Git]
tags: [development, git]
---

### System config location (.gitconfig file)

Linux systems: `/etc/gitconfig`
In the installation folder in Windows, ie. `C:/Program Files/Git/etc/gitconfig`

### Global config location

Home directory in Linux systems: `~/.gitconfig`
Windows: `%USERPROFILE%`

### Local config location

`.git/config` or `.git\config` in the current repository folder

### List the config values

We can add the `--show-origin` argument to see the file's location:

```bash
git config --list
```

List the global values only:

```bash
git config --global --list
```

List the local values only:

```bash
git config --local --list
```

### Add a config value

Example adding a user name (name is a property of the user's section) in the global section:

```bash
git config --global user.name Maxime Gault
```

### Remove a config value

A global one:

```bash
git config --global --unset http.sslBackend
```

A value can be configured in several files, the last one is used.

More info on [official website](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)
