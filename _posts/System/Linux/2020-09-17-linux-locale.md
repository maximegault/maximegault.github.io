---
title: Locale
date: 2020-09-17 15:30:00 HH:MM:SS +0200
categories: [System, Linux]
tags: [system, linux, shell, locale]
---

### See system locale

```bash
localectl
```

Output example:

```text
  System Locale: LANG=fr_FR.UTF-8
  VC Keymap: fr-oss
  X11 Layout: fr
  X11 Variant: oss
  changer la locale
```

### Change system locale

```bash
localectl set-locale LANG=en_US.UTF-8
localectl
```

Output example:

```text
   System Locale: LANG=en_US.UTF-8
   VC Keymap: fr-oss
   X11 Layout: fr
   X11 Variant: oss
```
