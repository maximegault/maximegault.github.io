---
title: PowerShell hashes
date: 2022-08-29 09:00:00 HH:MM:SS +0200
categories: [Development, PowerShell]
tags: [development, powershell, hashes]
---

### Create a hash

```powershell
$hash = @{};
```

### Add a key

```powershell
$hash.Add("newKey", 1);
```

### Modify an existing value

```powershell
$hash["existingKey"] = 2;
```

### Check if a key already exists

With `Contains`:

```powershell
$keyToTest = "keyToTest";
if ($hash.ContainsKey($keyToTest)) {
    $hash[$keyToTest]++;
}
else {
    $hash.Add($keyToTest, 1);
}
```

### Loop on the hash in an ordered way

With `getEnumerator`, ie. on the keys:

```powershell
$hash.getEnumerator() | Sort-Object Key | foreach {
    Write-Host -BackgroundColor Black -ForegroundColor Magenta ($_.key + ";" + $_.value);
}
```
