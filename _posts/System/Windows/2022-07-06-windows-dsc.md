---
title: Desired State Configuration
date: 2022-07-06 15:00:00 HH:MM:SS +0200
categories: [System, DSC]
tags: [system, dsc, tips]
---

## Desired State Configuration a.k.a. DSC

## Useful links

* [DSC home page](https://learn.microsoft.com/en-us/powershell/dsc/overview?view=dsc-1.1)
* [GitHub custom modules home page](https://github.com/dsccommunity)
* [xWebAdministration module](https://github.com/dsccommunity/xWebAdministration)

### List DSC resources present on the computer

With [`Get-DscResource`](https://docs.microsoft.com/en-us/powershell/module/psdesiredstateconfiguration/get-dscresource?view=dsc-2.0) cmdlet:

```powershell
Get-DscResource
```

Output:

```text
ImplementedAs   Name                      ModuleName                     Version    Properties
-------------   ----                      ----------                     -------    ----------
Binary          File                                                                {DestinationPath, Attributes, Ch...
Binary          SignatureValidation                                                 {SignedItemType, TrustedStorePath}
PowerShell      DefaultGatewayAddress     NetworkingDsc                  8.2.0      {AddressFamily, InterfaceAlias, ...
PowerShell      DnsClientGlobalSetting    NetworkingDsc                  8.2.0      {IsSingleInstance, DependsOn, De...
PowerShell      DnsConnectionSuffix       NetworkingDsc                  8.2.0      {ConnectionSpecificSuffix, Inter...
PowerShell      DnsServerAddress          NetworkingDsc                  8.2.0      {AddressFamily, InterfaceAlias, ...
PowerShell      Firewall                  NetworkingDsc                  8.2.0      {Name, Action, Authentication, D...
PowerShell      FirewallProfile           NetworkingDsc                  8.2.0      {Name, AllowInboundRules, AllowL...
PowerShell      HostsFile                 NetworkingDsc                  8.2.0      {HostName, DependsOn, Ensure, IP...
PowerShell      IPAddress                 NetworkingDsc                  8.2.0      {AddressFamily, InterfaceAlias, ...
PowerShell      IPAddressOption           NetworkingDsc                  8.2.0      {IPAddress, DependsOn, PsDscRunA...
PowerShell      NetAdapterAdvancedProp... NetworkingDsc                  8.2.0      {NetworkAdapterName, RegistryKey...
PowerShell      NetAdapterBinding         NetworkingDsc                  8.2.0      {ComponentId, InterfaceAlias, De...
PowerShell      NetAdapterLso             NetworkingDsc                  8.2.0      {Name, Protocol, State, DependsO...
PowerShell      NetAdapterName            NetworkingDsc                  8.2.0      {NewName, DependsOn, DriverDescr...
PowerShell      NetAdapterRdma            NetworkingDsc                  8.2.0      {Name, DependsOn, Enabled, PsDsc...
PowerShell      NetAdapterRsc             NetworkingDsc                  8.2.0      {Name, Protocol, State, DependsO...
PowerShell      NetAdapterRss             NetworkingDsc                  8.2.0      {Enabled, Name, DependsOn, PsDsc...
PowerShell      NetAdapterState           NetworkingDsc                  8.2.0      {Name, State, DependsOn, PsDscRu...
PowerShell      NetBios                   NetworkingDsc                  8.2.0      {InterfaceAlias, Setting, Depend...
PowerShell      NetConnectionProfile      NetworkingDsc                  8.2.0      {InterfaceAlias, DependsOn, IPv4...
PowerShell      NetIPInterface            NetworkingDsc                  8.2.0      {AddressFamily, InterfaceAlias, ...
PowerShell      NetworkTeam               NetworkingDsc                  8.2.0      {Name, TeamMembers, DependsOn, E...
PowerShell      NetworkTeamInterface      NetworkingDsc                  8.2.0      {Name, TeamName, DependsOn, Ensu...
PowerShell      ProxySettings             NetworkingDsc                  8.2.0      {IsSingleInstance, AutoConfigURL...
PowerShell      Route                     NetworkingDsc                  8.2.0      {AddressFamily, DestinationPrefi...
PowerShell      WaitForNetworkTeam        NetworkingDsc                  8.2.0      {Name, DependsOn, PsDscRunAsCred...
PowerShell      WinsServerAddress         NetworkingDsc                  8.2.0      {InterfaceAlias, Address, Depend...
PowerShell      WinsSetting               NetworkingDsc                  8.2.0      {IsSingleInstance, DependsOn, En...
PowerShell      PackageManagement         PackageManagement              1.0.0.1    {Name, AdditionalParameters, Dep...
PowerShell      PackageManagementSource   PackageManagement              1.0.0.1    {Name, ProviderName, SourceUri, ...
PowerShell      Archive                   PSDesiredStateConfiguration    1.1        {Destination, Path, Checksum, Cr...
PowerShell      Environment               PSDesiredStateConfiguration    1.1        {Name, DependsOn, Ensure, Path...}
PowerShell      Group                     PSDesiredStateConfiguration    1.1        {GroupName, Credential, DependsO...
Composite       GroupSet                  PSDesiredStateConfiguration    1.1        {DependsOn, PsDscRunAsCredential...
Binary          Log                       PSDesiredStateConfiguration    1.1        {Message, DependsOn, PsDscRunAsC...
PowerShell      Package                   PSDesiredStateConfiguration    1.1        {Name, Path, ProductId, Argument...
Composite       ProcessSet                PSDesiredStateConfiguration    1.1        {DependsOn, PsDscRunAsCredential...
PowerShell      Registry                  PSDesiredStateConfiguration    1.1        {Key, ValueName, DependsOn, Ensu...
PowerShell      Script                    PSDesiredStateConfiguration    1.1        {GetScript, SetScript, TestScrip...
PowerShell      Service                   PSDesiredStateConfiguration    1.1        {Name, BuiltInAccount, Credentia...
Composite       ServiceSet                PSDesiredStateConfiguration    1.1        {DependsOn, PsDscRunAsCredential...
PowerShell      User                      PSDesiredStateConfiguration    1.1        {UserName, DependsOn, Descriptio...
PowerShell      WaitForAll                PSDesiredStateConfiguration    1.1        {NodeName, ResourceName, Depends...
PowerShell      WaitForAny                PSDesiredStateConfiguration    1.1        {NodeName, ResourceName, Depends...
PowerShell      WaitForSome               PSDesiredStateConfiguration    1.1        {NodeCount, NodeName, ResourceNa...
PowerShell      WindowsFeature            PSDesiredStateConfiguration    1.1        {Name, Credential, DependsOn, En...
Composite       WindowsFeatureSet         PSDesiredStateConfiguration    1.1        {DependsOn, PsDscRunAsCredential...
PowerShell      WindowsOptionalFeature    PSDesiredStateConfiguration    1.1        {Name, DependsOn, Ensure, LogLev...
Composite       WindowsOptionalFeatureSet PSDesiredStateConfiguration    1.1        {DependsOn, PsDscRunAsCredential...
PowerShell      WindowsPackageCab         PSDesiredStateConfiguration    1.1        {Ensure, Name, SourcePath, Depen...
PowerShell      WindowsProcess            PSDesiredStateConfiguration    1.1        {Arguments, Path, Credential, De...
PowerShell      WebApplicationHandler     xwebadministration             3.2.0      {Name, Path, AllowPathInfo, Depe...
PowerShell      xIisFeatureDelegation     xwebadministration             3.2.0      {Filter, OverrideMode, Path, Dep...
PowerShell      xIisHandler               xwebadministration             3.2.0      {Ensure, Name, DependsOn, PsDscR...
PowerShell      xIisLogging               xwebadministration             3.2.0      {LogPath, DependsOn, LogCustomFi...
PowerShell      xIisMimeTypeMapping       xwebadministration             3.2.0      {ConfigurationPath, Ensure, Exte...
PowerShell      xIisModule                xwebadministration             3.2.0      {Name, Path, RequestPath, Verb...}
PowerShell      xSslSettings              xwebadministration             3.2.0      {Bindings, Name, DependsOn, Ensu...
PowerShell      xWebApplication           xwebadministration             3.2.0      {Name, PhysicalPath, WebAppPool,...
PowerShell      xWebAppPool               xwebadministration             3.2.0      {Name, autoShutdownExe, autoShut...
PowerShell      xWebAppPoolDefaults       xwebadministration             3.2.0      {IsSingleInstance, DependsOn, Id...
PowerShell      xWebConfigKeyValue        xwebadministration             3.2.0      {ConfigSection, Key, WebsitePath...
PowerShell      xWebConfigProperty        xwebadministration             3.2.0      {Filter, PropertyName, WebsitePa...
PowerShell      xWebConfigPropertyColl... xwebadministration             3.2.0      {CollectionName, Filter, ItemKey...
PowerShell      xWebSite                  xwebadministration             3.2.0      {Name, ApplicationPool, Applicat...
PowerShell      xWebSiteDefaults          xwebadministration             3.2.0      {IsSingleInstance, AllowSubDirCo...
PowerShell      xWebVirtualDirectory      xwebadministration             3.2.0      {Name, PhysicalPath, WebApplicat...
```

### Get a single DSC module's resources details

With [`Get-DscResource`](https://docs.microsoft.com/en-us/powershell/module/psdesiredstateconfiguration/get-dscresource?view=dsc-2.0) cmdlet and the `-Module` argument:

```powershell
Get-DscResource -Module xWebAdministration
```

Output:

```text
ImplementedAs   Name                      ModuleName                     Version    Properties
-------------   ----                      ----------                     -------    ----------
PowerShell      WebApplicationHandler     xwebadministration             3.2.0      {Name, Path, AllowPathInfo, DependsOn...}
PowerShell      xIisFeatureDelegation     xwebadministration             3.2.0      {Filter, OverrideMode, Path, DependsOn...}
PowerShell      xIisHandler               xwebadministration             3.2.0      {Ensure, Name, DependsOn, PsDscRunAsCredential}
PowerShell      xIisLogging               xwebadministration             3.2.0      {LogPath, DependsOn, LogCustomFields, LogFlags...}
PowerShell      xIisMimeTypeMapping       xwebadministration             3.2.0      {ConfigurationPath, Ensure, Extension, MimeType...
PowerShell      xIisModule                xwebadministration             3.2.0      {Name, Path, RequestPath, Verb...}
PowerShell      xSslSettings              xwebadministration             3.2.0      {Bindings, Name, DependsOn, Ensure...}
PowerShell      xWebApplication           xwebadministration             3.2.0      {Name, PhysicalPath, WebAppPool, Website...}
PowerShell      xWebAppPool               xwebadministration             3.2.0      {Name, autoShutdownExe, autoShutdownParams, aut...
PowerShell      xWebAppPoolDefaults       xwebadministration             3.2.0      {IsSingleInstance, DependsOn, IdentityType, Man...
PowerShell      xWebConfigKeyValue        xwebadministration             3.2.0      {ConfigSection, Key, WebsitePath, DependsOn...}
PowerShell      xWebConfigProperty        xwebadministration             3.2.0      {Filter, PropertyName, WebsitePath, DependsOn...}
PowerShell      xWebConfigPropertyColl... xwebadministration             3.2.0      {CollectionName, Filter, ItemKeyName, ItemKeyVa...
PowerShell      xWebSite                  xwebadministration             3.2.0      {Name, ApplicationPool, ApplicationType, Authen...
PowerShell      xWebSiteDefaults          xwebadministration             3.2.0      {IsSingleInstance, AllowSubDirConfig, DefaultAp...
PowerShell      xWebVirtualDirectory      xwebadministration             3.2.0      {Name, PhysicalPath, WebApplication, Website...}
```

### Get a single DSC resource details

With [`Get-DscResource`](https://docs.microsoft.com/en-us/powershell/module/psdesiredstateconfiguration/get-dscresource?view=dsc-2.0) cmdlet and the `-Name` and `-Syntax` arguments:

```powershell
Get-DscResource -Name xWebSite  -Syntax
```

Output:

```text
xWebSite [String] #ResourceName
{
    Name = [string]
    [ApplicationPool = [string]]
    [ApplicationType = [string]]
    [AuthenticationInfo = [MSFT_xWebAuthenticationInformation]]
    [BindingInfo = [MSFT_xWebBindingInformation[]]]
    [DefaultPage = [string[]]]
    [DependsOn = [string[]]]
    [EnabledProtocols = [string]]
    [Ensure = [string]{ Absent | Present }]
    [LogCustomFields = [MSFT_xLogCustomFieldInformation[]]]
    [LogFlags = [string[]]{ BytesRecv | BytesSent | ClientIP | ComputerName | Cookie | Date | Host | HttpStatus | HttpSubStatus | Method | ProtocolVersion | Referer | ServerIP | ServerPort | SiteName | Time | TimeTaken | UriQuery | UriStem | UserAgent | UserName | Win32Status }]
    [LogFormat = [string]{ IIS | NCSA | W3C }]
    [LoglocalTimeRollover = [bool]]
    [LogPath = [string]]
    [LogPeriod = [string]{ Daily | Hourly | MaxSize | Monthly | Weekly }]
    [LogTargetW3C = [string]{ ETW | File | File,ETW }]
    [LogTruncateSize = [string]]
    [PhysicalPath = [string]]
    [PreloadEnabled = [bool]]
    [PsDscRunAsCredential = [PSCredential]]
    [ServerAutoStart = [bool]]
    [ServiceAutoStartEnabled = [bool]]
    [ServiceAutoStartProvider = [string]]
    [SiteId = [UInt32]]
    [State = [string]{ Started | Stopped }]
}
```

### Test a DSC configuration

Test configurations specified in a folder with the [`Test-DscConfiguration`](https://docs.microsoft.com/en-us/powershell/module/psdesiredstateconfiguration/Test-DSCConfiguration?view=dsc-1.1) cmdlet and the `-Path` argument:

```powershell
Test-DscConfiguration -Path myPath
```

Output:

```text
PSComputerName  ResourcesInDesiredState        ResourcesNotInDesiredState     InDesiredState 
--------------  -----------------------        --------------------------     -------------- 
localhost       {[WindowsFeature]IIS, [Wind... {[WindowsFeature]AspNet45, ... False          
```

Or:

```text
InDesiredState             : False
ResourcesInDesiredState    : {[WindowsFeature]IIS, [WindowsFeature]WebServerManagementConsole}
ResourcesNotInDesiredState : {[WindowsFeature]AspNet45, [xWebSite]DefaultSite}
ReturnValue                : 0
PSComputerName             : localhost
```

### List features

With [`Get-WindowsFeature`](https://docs.microsoft.com/en-us/powershell/module/servermanager/get-windowsfeature?view=windowsserver2022-ps):

```powershell
Get-WindowsFeature
```

Output:

```text
Display Name                                            Name                       Install State
------------                                            ----                       -------------
[ ] Active Directory Certificate Services               AD-Certificate                 Available
    [ ] Certification Authority                         ADCS-Cert-Authority            Available
    [ ] Certificate Enrollment Policy Web Service       ADCS-Enroll-Web-Pol            Available
    [ ] Certificate Enrollment Web Service              ADCS-Enroll-Web-Svc            Available
    [ ] Certification Authority Web Enrollment          ADCS-Web-Enrollment            Available
    [ ] Network Device Enrollment Service               ADCS-Device-Enrollment         Available
    [ ] Online Responder                                ADCS-Online-Cert               Available
[ ] Active Directory Domain Services                    AD-Domain-Services             Available
[ ] Active Directory Federation Services                ADFS-Federation                Available
[ ] Active Directory Lightweight Directory Services     ADLDS                          Available
[ ] Active Directory Rights Management Services         ADRMS                          Available
    [ ] Active Directory Rights Management Server       ADRMS-Server                   Available
    [ ] Identity Federation Support                     ADRMS-Identity                 Available
[ ] Device Health Attestation                           DeviceHealthAttestat...        Available
[ ] DHCP Server                                         DHCP                           Available
[ ] DNS Server                                          DNS                            Available
[ ] Fax Server                                          Fax                            Available
[X] File and Storage Services                           FileAndStorage-Services        Installed
    [ ] File and iSCSI Services                         File-Services                  Available
        [ ] File Server                                 FS-FileServer                  Available
...
```

Can also be rendered with a gridview thanks to [`Out-GridView`](https://docs.microsoft.com/fr-fr/powershell/module/microsoft.powershell.utility/out-gridview?view=powershell-7.2):

```powershell
Get-WindowsFeature | Out-GridView
```

### Install a feature

With [`WindowsFeature`](https://learn.microsoft.com/en-us/powershell/dsc/reference/resources/windows/windowsfeatureresource?view=dsc-1.1):

Examples:

```powershell
$titleIISRole = 'IIS role'
$titleWindowsFeature = '[WindowsFeature]'
$windowsFeatureIIS = "$titleWindowsFeature$titleIISRole"
$statePresent = 'Present'

# ...

# Install the IIS role
WindowsFeature $titleIISRole {
    Ensure                          = $statePresent
    Name                            = 'Web-Server'
}

# Install the ASP .NET 4.7 role (yes the role's name ends with 45...)
WindowsFeature "ASP .NET 4.7 role" {
    Ensure                          = $statePresent
    Name                            = 'Web-Asp-Net45'
    # It adds '[WindowsFeature]' as a prefix before the given title (same for each component)
    DependsOn                       = $windowsFeatureIIS
}
```

### Create a web site

With `xWebsite`:

Example:

```powershell
$defaultWebSiteName = 'Default Web Site'
$defaultWebSitePath = 'C:\inetpub\wwwroot'
$statePresent = 'Present'
$stateStarted = 'Started'
$titleWindowsFeature = '[WindowsFeature]'
$windowsFeatureIIS = "$titleWindowsFeature$titleIISRole"

# ...

# Default Web Site settings
xWebsite $defaultWebSiteName {
    Ensure                          = $statePresent
    Name                            = $defaultWebSiteName
    State                           = $stateStarted
    PhysicalPath                    = $defaultWebSitePath
    BindingInfo                     = @(
        MSFT_xWebBindingInformation {
            Protocol                = "http"
            Port                    = 80
            HostName                = 'a.binding.myHost.com'
        },
        MSFT_xWebBindingInformation {
            Protocol                = "https"
            Port                    = 443
            HostName                = 'another.binding.myHost.com'
            CertificateThumbprint   = 'aCertificateThumbrprint'
        }
    )
    DependsOn                       = $windowsFeatureIIS
}
```

### Create a virtual directory

With `xWebVirtualDirectory`:

Example:

```powershell
$defaultWebSiteName = 'Default Web Site'
$titleXWebsite = '[xWebsite]'
$xWebSiteDefaultWebSite = "$titleXWebsite$defaultWebSiteName"
$statePresent = 'Present'
$myDirectory = 'myDirectory'
$mainVirtualDirectoryPath = Join-Path 'E:\IIS\Websites' -ChildPath $myDirectory

# ...

xWebVirtualDirectory $myDirectory {
    Name                            = $myDirectory
    PhysicalPath                    = $mainVirtualDirectoryPath
    WebApplication                  = ''
    Website                         = $defaultWebSiteName
    Ensure                          = $statePresent
    DependsOn                       = $xWebSiteDefaultWebSite
}
```

### Create an application pool

With `xWebAppPool`:

Example:

```powershell
$defaultWebSiteName = 'Default Web Site'
$defaultWebSitePath = 'C:\inetpub\wwwroot'
$statePresent = 'Present'
$stateStarted = 'Started'
$titleWindowsFeature = '[WindowsFeature]'
$windowsFeatureIIS = "$titleWindowsFeature$titleIISRole"
$applicationPoolName = 'My_Application_Pool'
$poolGroupUser = "IIS APPPOOL\$applicationPoolName"
$applicationPoolIdentity = 'ApplicationPoolIdentity'

# ...

# Create a new application pool for the application
xWebAppPool $applicationPoolName {
    Name                            = $applicationPoolName
    Ensure                          = $statePresent
    DependsOn                       = $windowsFeatureIIS
    # https://docs.microsoft.com/en-us/iis/configuration/system.applicationhost/applicationpools/applicationpooldefaults/
    # This autoStart value can possibly not appear in the final file because its default value is true
    autoStart                       = $true
    # This managedRuntimeVersion value can possibly not appear in the final file if the same value is defined in the applicationPoolDefaults block (here v4.0)
    managedRuntimeVersion           = $version4_0
    startMode                       = $alwaysRunning
    # This identityType value can possibly not appear in the final file if the same value is defined in the applicationPoolDefaults block (here ApplicationPoolIdentity)
    identityType                    = $applicationPoolIdentity
    loadUserProfile                 = $true
    idleTimeout                     = "00:00:00"
    maxProcesses                    = 2
    # https://docs.microsoft.com/en-us/iis/configuration/system.applicationhost/applicationpools/add/recycling/
    # This disallowOverlappingRotation value can possibly not appear in the final file because its default value is false
    disallowOverlappingRotation     = $false
    disallowRotationOnConfigChange  = $true
    logEventOnRecycle               = "$memory, $configChange, $privateMemory"
    restartRequestsLimit            = 2000
    restartTimeLimit                = "02:00:00"
    rapidFailProtectionInterval     = "00:01:00"
    rapidFailProtectionMaxCrashes   = 20
    # https://docs.microsoft.com/en-us/iis/configuration/system.applicationhost/applicationpools/add/recycling/periodicrestart/schedule/
    # No restart schedule
}
```

### Create a web application

With `xWebApplication`:

Example:

```powershell
$defaultWebSiteName = 'Default Web Site'
$defaultWebSitePath = 'C:\inetpub\wwwroot'
$statePresent = 'Present'
$stateStarted = 'Started'
$titleWindowsFeature = '[WindowsFeature]'
$windowsFeatureIIS = "$titleWindowsFeature$titleIISRole"

$webAppPath = '...'
$myDirectory = 'myDirectory'
$applicationPoolName = 'My_Application_Pool'
# "DependsOn" does not accept dependances named with slashes
$iisRelativeNoVersionPath = "$myDirectory/MyService"
$titleIisRelativeNoVersionPath = $iisRelativeNoVersionPath.Replace("/", "_")
$titleIisRelativeVersionPath = $iisRelativeVersionPath.Replace("/", "_")

# ...

# Default Web Site settings
xWebApplication $titleIisRelativeVersionPath {
    Name                            = $iisRelativeVersionPath
    PhysicalPath                    = $webAppPath
    Website                         = $defaultWebSiteName
    WebAppPool                      = $applicationPoolName
    Ensure                          = $statePresent
    # To use SSL
    # SslFlags                        = $ssl
    SslFlags                        = ''
    ServiceAutoStartEnabled         = $true
    ApplicationType                 = $applicationType
    ServiceAutoStartProvider        = $serviceAutoStartProvider
    DependsOn                       = $xWebVirtualDirectoryRelativeNoVersionPath, $xWebAppPoolApplicationPoolName
}
```

### Change hosts file

With the [`NetworkingDsc`](https://github.com/dsccommunity/NetworkingDsc) module and `HostsFile`:

Example:

```powershell
Import-DscResource -Module NetworkingDsc

# ...
HostsFile "Modifying the hosts" {
    HostName = "myNewHost.myHost.com"
    IPAddress = "127.0.0.1"
    Ensure = "Present"
}
```
