---
title: Conditions and tests
date: 2020-10-29 09:30:00 HH:MM:SS +0100
categories: [System, Linux, Bash, Shell]
tags: [system, linux, bash, shell, tests, conditions]
---

### Conditions

* `-d` returns true if a directory exists
* `-e` returns true if a file exists
* `-o` perform a logical or between two tests
* `-z` returns true if the given command returns an empty string
* `-L` checks if a symlink exists
  
```bash
if [ -z "`grep "DIRECTORY" /etc/sudoers`" ]
```

[GNU conditions manpage](https://linux.die.net/man/1/test)
[BSD conditions manpage](https://www.freebsd.org/cgi/man.cgi?test)
[More conditions](https://fr.wikibooks.org/wiki/Programmation_Bash/Tests)

### Test command

* `&&`
  
```bash
test cmd1 && cmd2
```

cmd1 is executed, if its return code equals 0 then cmd2 is executed

* `||`

```bash
test cmd1 || cmd2
```

cmd1 is executed, if its return code is different than 0 then cmd2 is executed

[test command](https://en.wikipedia.org/wiki/Test_(Unix))
