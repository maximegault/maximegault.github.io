---
title: Tips
date: 2020-09-18 14:00:00 HH:MM:SS +0200
categories: [System, Linux]
tags: [system, linux, shell, tips, symlink, pwd, printenv, env, set, unset, password, sudo, sudoers, user, group, find]
---

### Redirections

Use `>` to redirect an output to a file (`>>` to append).

To redirect special handles:

* file descriptor 0 is the standard input (`stdin`)
* file descriptor 1 is the standard output (`stdout`)
* file descriptor 2 is the standard error (`stderr`)

So ([source](https://askubuntu.com/a/350216)):

* `> file` redirects stdout to file
* `1> file` redirects stdout to file
* `2> file` redirects stderr to file
* `&> file` redirects stdout and stderr to file (equivalent of older `2>&1 > file`)
* `> file 2>&1` redirects stdout and stderr to file

Examples:

`myCommand > myFile` (equivalent of `myCommand 1> myFile`) will redirect its standard output to `myFile`
`myCommand 2> myFile` will redirect its standard error to `myFile`
`myCommand 2>&1 > myFile` will redirect its standard error to its standard output, then its standard output to `myFile`

Warning: `2>1` would result in a `1` file creation, the `&` is important and indicates that what is next is a file descriptor and not a file.

`/dev/null` is the special "null device" used to redirect a useless content, ie. `myCommand 2>&1 /dev/null`.

### Find something

```bash
find <base> -name <name>
```

Example:

```bash
find / -name *.py
```

```bash
find / -name *.py 2>/dev/null
```

### Create a symlink

```bash
ln -s ~/foo/bar/baz ~/mySymLink
```

### Print Working Directory a.k.a. pwd

When you've followed a symlink, pwd shows by default the logical path, ie:

```bash
cd ~/mySymLink
pwd
```

```text
~/mySymLink
```

If you need the real physical path, use the -P argument:

```bash
pwd -P
```

```text
~/foo/bar/baz
```

### Environment variables

List them:

```bash
printenv 
```

Add one:

```bash
export MY_VAR 
```

Display one:

```bash
echo $MY_VAR 
```

Remove one:

```bash
unset MY_VAR 
```

### Read a password from the console and avoid to have it in the bash_history

Example setting a proxy in an environment variable:

```bash
read -s pass
export my_var="http://user:$pass@my_proxy.com:1234"
```

### Validate a sudoers file

For a user `user` that has a specific sudoers `/etc/sudoers.d/user` file:

```bash
visudo -cf /etc/sudoers.d/user
```

Error output example:

```bash
>>> /etc/sudoers.d/user: syntax error near line 2 <<<
parse error in /etc/sudoers.d/user near line 2
```

OK output example:

```bash
/etc/sudoers.d/user: parsed OK
```

### Watch

The `watch` command executes a program periodically, showing output fullscreen

```bash
watch 'ps -ef | grep slapd'
```

```text
Every 2.0s: ps -ef | grep slapd            myServer: Thu Nov 25 09:26:31 2021

rhds        1489       1  0 Nov11 ?        02:19:10 /usr/sbin/ns-slapd -D /etc/mydirsrv/slapd-01 -i /run/mydirsrv/slapd-01.pid
root      901137  900625  0 09:26 pts/1    00:00:00 watch ps -ef | grep slapd
root      901156  901137  0 09:26 pts/1    00:00:00 watch ps -ef | grep slapd
root      901157  901156  0 09:26 pts/1    00:00:00 sh -c ps -ef | grep slapd
root      901159  901157  0 09:26 pts/1    00:00:00 grep slapd
```

### netstat

Continuous netstat:

```bash
netstat -c
```

See opened connections, with:

* `-t`: TCP
* `-u`: UDP
* `-a`: all (to include sockets in `listen` state)
  
```bash
netstat -a
```

### Colors

Show colors on a `ls` with the argument `--color=auto`:

```bash
ls --color=auto
```

So the following permanent aliases can be added in the `.bash_aliases` file:

```bash
alias ls='ls --color=auto'
alias ll='ls -l'
alias la='ls -la'
```

### Tar

Archive and compress (in the same time, so create a `tgz`) a folder:

```bash
tar cfvz myDirectoryArchived.tgz myDirectory/
```

### Grep

Show lines:

* before the match with `-B`
* after the match with `-A`

```bash
grep -B 5 -A 3 'word' fileToLookIn
```

Color the needed word with `--color`:

```bash
grep --color 'word' fileToLookIn
```

Recursive in a directory with `-r`:

```bash
grep -r 'word' directoryToLookIn/*
```

### Epoch

[Epoch converter](https://www.epochconverter.com/)
