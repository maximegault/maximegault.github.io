---
title: PowerShell dates
date: 2022-08-29 09:00:00 HH:MM:SS +0200
categories: [Development, PowerShell]
tags: [development, powershell, dates]
---

### Get the number of days in a month thanks to its number

With `DaysInMonth`:

```powershell
$currentMonth = 7
$currentLastDay = [DateTime]::DaysInMonth([DateTime]::Now.Year, $currentMonth);
# $currentLastDay is 31
```

### Get a month's name thanks to its number

With `GetMonthName`:

```powershell
$currentMonth = 7
(Get-Culture).DateTimeFormat.GetMonthName($currentMonth)
```

Output is:

```text
July
```
