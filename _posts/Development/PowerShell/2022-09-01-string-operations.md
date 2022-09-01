---
title: PowerShell string operations
date: 2022-08-29 09:00:00 HH:MM:SS +0200
categories: [Development, PowerShell]
tags: [development, powershell, string]
---

### Grep like

#### Simple grep

With the [Select-String](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-string?view=powershell-7) cmdlet. Ie.:

```powershell
Select-String "Pattern to look for" "C:\myDirectory\*.txt"
```

Output is like:

```text
C:\myDirectory\one.txt:AAA Pattern to look for BBB
C:\myDirectory\two.txt:AAA Pattern to look for BBB
```

#### With a lines count (`wc -l` equivalent)

With the [Measure-Object](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/measure-object?view=powershell-7) cmdlet. Ie.:

```powershell
Select-String "Pattern to look for" "C:\myDirectory\*.txt" | Measure-Object -Line
```

Output is like:

```text
Lines Words Characters Property
----- ----- ---------- --------
  836                          
 1698
```

Select only the lines count with [Select-Object](https://docs.microsoft.com/fr-fr/powershell/module/microsoft.powershell.utility/select-object?view=powershell-7.2) `-ExpandProperty` argument:

```powershell
Select-String "Pattern to look for" "C:\myDirectory\*.txt" | Measure-Object -Line | Select-Object -ExpandProperty Lines
```

Output is like:

```text
836                          
1698
```

#### Grep with regex extraction on output

With the [Select-String](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-string?view=powershell-7) cmdlet again. Ie.:

```powershell
$regex = [regex]"^.*Foo_Bar(.*?)Baz_Qux$";
# We get every match in each line with -AllMatches (without it, it will get the first match in each line)
Select-String -Path "C:\myDirectory\*.txt" $regex -AllMatches | Foreach-Object {$_.Matches} | Foreach-Object {
    $currentNeededPart = $_.Groups[1].Value;
};
```
