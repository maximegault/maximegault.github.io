---
title: Powershell tips
date: 2020-10-29 11:30:00 HH:MM:SS +0100
categories: [Development, PowerShell]
tags: [development, powershell, tips]
---

### Change the console title

Very useful when there are many windows opened (or in a script):

```powershell
$title = "My new awesome title"
$host.UI.RawUI.WindowTitle = $title
```

### List profile files

`profile.ps1` is a pseudo equivalent of Linux's `.bashrc`, it's user's specific configuration. It has different levels: to see all files:

```powershell
$PROFILE | Get-Member -MemberType noteproperty | Format-List
```

Output example:

```text
TypeName   : System.String
Name       : AllUsersAllHosts
MemberType : NoteProperty
Definition : string AllUsersAllHosts=C:\Windows\System32\WindowsPowerShell\v1.0\profile.ps1

TypeName   : System.String
Name       : AllUsersCurrentHost
MemberType : NoteProperty
Definition : string AllUsersCurrentHost=C:\Windows\System32\WindowsPowerShell\v1.0\Microsoft.PowerShell_profile.ps1

TypeName   : System.String
Name       : CurrentUserAllHosts
MemberType : NoteProperty
Definition : string CurrentUserAllHosts=C:\Users\myUser\Documents\WindowsPowerShell\profile.ps1

TypeName   : System.String
Name       : CurrentUserCurrentHost
MemberType : NoteProperty
Definition : string CurrentUserCurrentHost=C:\Users\myUser\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
```

### Add a permanent alias

Through the `profile.ps1`, example:

```powershell
Set-Alias ll Get-ChildItem
```

[Set-Alias documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-7.2)

### Escape quotes

Variables need to be in double quotes `""` to be interpreted, ie:

```powershell
$foo = "FOO"
$bar = '$foo'
$baz = "$foo"
Write-Host $bar
Write-Host $baz
```

Output will be:

```text
$foo
FOO
```

So if we need a double quote insided a double quoted string, we can escape them with `` "`" ``:

```powershell
$foo = "FOO"
$bar = "Hello `"$foo`""
Write-Host $bar
```

Output will be:

```text
Hello "FOO"
```

### Get command output

Simply call ithe command the easiest way and put it in a variable:

```powershell
$jwt = java -jar myJar.jar argument
Write-Host $jwt
```

### Copy to the clipboard

Use the `Set-Clipboard` cmdlet with the `-Value` argument:

```powershell
$jwt = java -jar myJar.jar argument
Set-Clipboard -Value $jwt
```

[Set-Clipboard documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/set-clipboard?view=powershell-7.2)

### Get a certificate thumbprint

Example with a certificate which subject is `CN=HOSTNAME` (aka the self-signed certificate):

```powershell
$hostname = hostname
$certificateStorePath = "Cert:\LocalMachine\My"
$serverCertificateThumbprint = Get-ChildItem $certificateStorePath | Where-Object Subject -eq "CN=$hostname" | Select-Object -ExpandProperty Thumbprint
```

Other method:

```powershell
$certificateStorePath = "Cert:\LocalMachine\My"
$serverCertificateThumbprint = Get-ChildItem $certificateStorePath | Where-Object {$_.Subject -eq "CN=$($env:COMPUTERNAME)"} | Select-Object -ExpandProperty Thumbprint
```

[Select-Object documentation](https://docs.microsoft.com/fr-fr/powershell/module/microsoft.powershell.utility/select-object?view=powershell-7.2)

### Give read rights on this certificate

[Source](https://stackoverflow.com/a/71069993/7445285)

```powershell
$hostname = hostname
$certificateStorePath = "Cert:\LocalMachine\My"
$certificateThumbprint = 'abcdefb5956babcdefa71dabcdeffac5e5abcdef'
# Looking for the needed certificate
$certificate = Get-ChildItem $certificateStorePath | Where-Object Thumbprint -eq $certificateThumbprint
# Getting its private key
$privateKey = [System.Security.Cryptography.X509Certificates.RSACertificateExtensions]::GetRSAPrivateKey($certificate)
$containerName = ''
if ($privateKey.GetType().Name -ieq "RSACng") {
    $containerName = $privateKey.Key.UniqueName
}
else {
    $containerName = $privateKey.CspKeyContainerInfo.UniqueKeyContainerName
}    
$keyFullPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $containerName;
if (-Not (Test-Path -Path $keyFullPath -PathType Leaf)) {
    throw "Unable to get the private key container to set permissions."
}
# Get the current ACL of the private key
$acl = (Get-Item $keyFullPath).GetAccessControl()    
# Add the new ACE to the ACL of the private key
# Imagine we give rights to two groups
$certificateGroup = 'IIS_APPPOOLS_certificate_user'
$iisGroup = 'IIS_IUSRS'
$accessRulePoolGroup = New-Object System.Security.AccessControl.FileSystemAccessRule($certificateGroup, "Read", "Allow")
$accessRuleIisGroup = New-Object System.Security.AccessControl.FileSystemAccessRule($iisGroup, "Read", "Allow")
$acl.AddAccessRule($accessRulePoolGroup);
$acl.AddAccessRule($accessRuleIisGroup);
# Write back the new ACL
Set-Acl -Path $keyFullPath -AclObject $acl;
```

### Compute a file hash

With the [Get-FileHash](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-7.2) cmdlet. Default hash algorithm (if not specified with `-Algorithm` argument) is `SHA256`.

To get a default `SHA256` hash:

```powershell
Get-FileHash C:\myFile.txt
```

Output is:

```text
Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA256          BFF9360D9A14AAE7B8CC4D829C6570B3FA72925E0ECF037582DEAEAEAA8E268C       C:\myFile.txt
```

To get a `MD5` hash and display only the hash:

```powershell
Get-FileHash C:\myFile.txt -Algorithm MD5 | Select-Object Hash
```

Output is:

```text
Hash
----
BFF9360D9A14AAE7B8CC4D829C6570B3FA72925E0ECF037582DEAEAEAA8E268C
```

To get a `MD5` hash and get only the hash:

```powershell
Get-FileHash C:\myFile.txt -Algorithm MD5 | Select-Object -ExpandProperty Hash
```

Output is:

```text
BFF9360D9A14AAE7B8CC4D829C6570B3FA72925E0ECF037582DEAEAEAA8E268C
```

### Ternary operation

Like the traditional `condition ? true : false`:

```powershell
$foo = "Fake foo"
$bar = if ($foo -eq "Foo") { "It's foo!" } else { "It's not foo..." }
Write-Host $bar
```

Output:

````text
It's not foo...
```
