---
title: Path operations
date: 2020-08-07 16:00:00 HH:MM:SS +0200
categories: [Development, PowerShell]
tags: [development, powershell]     # TAG names should always be lowercase
---

### Joining two paths

```powershell
$basePath = 'C:\foo'
$firstChildPath = 'bar'
$secondChildPath = 'baz'
$fullPath = Join-Path $basePath -ChildPath $firstChildPath
$pipedFullPath = Join-Path $basePath -ChildPath $firstChildPath | Join-Path -Child-Path $secondChildPath
# fullPath is 'C:\foo\bar'
# pipedFullPath is 'C:\foo\bar\baz'
```

### Get parent path

```powershell
$fullPath = 'C:\foo\bar\baz'
$parentPath = Split-Path $fullPath -Parent
$pipedParentPath = Split-Path $fullPath -Parent | Split-Path -Parent
# parentPath is 'C:\foo\bar'
# pipedParentPath is 'C:\foo'
```

### Get leaf

```powershell
$fullPath = 'C:\foo\bar\baz'
$leaf = Split-Path $fullPath -Leaf
# leaf is 'baz'
```

### Mix everything

```powershell
$fullPath = 'C:\foo\bar\baz'
$anotherLeaf = 'qux'
$mixedPipedPath = Split-Path $fullPath -Parent | Join-Path -Child-Path $anotherLeaf
# mixedPipedPath is 'C:\foo\bar\qux'
```

### Check if a path exist or not

With the [`Test-Path`](https://docs.microsoft.com/fr-fr/powershell/module/microsoft.powershell.management/test-path?view=powershell-7.2) cmdlet. To test its existence:

```powershell
if (Test-Path -Path 'myPath') {
    "Path exists!"
}
```

The contrary:

```powershell
if (!(Test-Path -Path 'myPath')) {
    "Path does not exist!"
}
```

### Create a directory

With the [New-Item](https://docs.microsoft.com/en-us/powershell/module/Microsoft.PowerShell.Management/New-Item?view=powershell-7.2) cmdlet:

```powershell
New-Item -ItemType Directory -Path 'myPath'
```
