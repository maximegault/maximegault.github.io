---
title: Powershell system
date: 2022-05-31 15:00:00 HH:MM:SS +0200
categories: [System, PowerShell]
tags: [system, powershell, tips]
---

## System commands

### Users and groups

#### Create a local group

With the [`New-LocalGroup`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/new-localgroup?view=powershell-5.1) cmdlet:

```powershell
New-LocalGroup -Name "MySecurityGroup"
```

#### Check if a local group exist

With the [`Get-LocalGroup`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/get-localgroup?view=powershell-5.1) cmdlet:

```powershell
$myGroup =  "MySecurityGroup"
try {
    Get-LocalGroup -Name "$myGroup" -ErrorAction Stop
    Write-Host "myGroup $myGroup found!"
}
catch {
    Write-Host "myGroup $myGroup does not exist"
}
```

#### Check if a local group contains a member

With the [`Get-LocalGroupMember`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/get-localgroupmember?view=powershell-5.1) cmdlet:

```powershell
try {
    Get-LocalGroupMember -Group "$myGroup" -Member $myUser -ErrorAction Stop
    Write-Host "Group $myGroup already contains user $myUser!"
}
catch {
    Write-Host "Group $myGroup does not contain user $myUser"
}
```

#### Add a user to a local group

With the [`Add-LocalGroupMember`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/add-localgroupmember?view=powershell-5.1) cmdlet:

```powershell
Add-LocalGroupMember -Group "MySecurityGroup" -Member "MyMember"
```

### Telnet alternative

With `Test-NetConnection` cmdlet:

```powershell
Test-NetConnection -computername myHost.com -port 1234
```

Output:

```text
ComputerName     : myHost.com
RemoteAddress    : 111.222.333.444
RemotePort       : 1234
InterfaceAlias   : Prod_1
SourceAddress    : 111.333.222.444
TcpTestSucceeded : True
```

For a SQL Server connection, test the `58227` port.

### wget alternative

With `Invoke-WebRequest` cmdlet:

```powershell
Invoke-WebRequest https://myHost.com/foo/bar
```

Output:

```text
StatusCode        : 200
StatusDescription : OK
Content           : {"someJSONKey":"someJSONValue"}
RawContent        : HTTP/1.1 200 OK
                    x-xss-protection: 0
                    content-security-policy: frame-ancestors 'self'                    
                    vary: Origin
                    x-content...
Forms             : {}
Headers           : {[x-xss-protection, 0], [content-security-policy, frame-ancestors 'self'], [expect-ct, max-age=0], [vary, Origin]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : System.__ComObject
RawContentLength  : 1940
```

## IIS

### Create a shared configuration

With [`Enable-IISSharedConfig`](https://docs.microsoft.com/en-us/powershell/module/iisadministration/enable-iissharedconfig?view=windowsserver2022-ps) and [`Export-IISConfiguration`](https://docs.microsoft.com/en-us/powershell/module/iisadministration/export-iisconfiguration?view=windowsserver2022-ps):

```powershell
# Configure shared configuration (even if alone, simpler to edit)*
$IISSharedConfigPath = "sharedPath"
$KeyEncryptionPasswordPlainText = "password"

New-Item -Path $IISSharedConfigPath -ItemType Directory -Force
$KeyEncryptionPassword = ConvertTo-SecureString -AsPlainText -String $KeyEncryptionPasswordPlainText -Force
Export-IISConfiguration -PhysicalPath $IISSharedConfigPath -KeyEncryptionPassword $KeyEncryptionPassword
Enable-IISSharedConfig -PhysicalPath $IISSharedConfigPath -KeyEncryptionPassword $KeyEncryptionPassword
```

### Disable a shared configuration

With [`Disable-IISSharedConfig`](https://docs.microsoft.com/en-us/powershell/module/iisadministration/disable-iissharedconfig?view=windowsserver2022-ps):

```powershell
Disable-IISSharedConfig
```
