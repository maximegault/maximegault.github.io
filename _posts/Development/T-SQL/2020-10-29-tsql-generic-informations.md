---
title: Generic informations
date: 2020-10-29 08:30:00 HH:MM:SS +0100
categories: [Development, T-SQL, PowerShell]
tags: [development, t-sql, sqlserver, database, column, powershell]
---

### Check if a connection is OK or not

```powershell
$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = 'data source=mySource;initial catalog=myDatabse;persist security info=True;user id=myUser;password=myPassword;MultipleActiveResultSets=True;App=EntityFramework'
$connection.Open()
$connection.Close()
```

No output if the connection is OK, otherwise there will be an error on stderr.

### List installed instances with PowerShell

On the server, querying the registry with [`Get-ItemProperty`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-itemproperty?view=powershell-7.2):

```powershell
$instances = (Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\Microsoft SQL Server').InstalledInstances
Write-Host $instances 
```

Output:

```text
MYINSTANCE1
MYINSTANCE2
```

### List all databases

Perform a query on the `master.dbo.sysdatabases` table:

```sql
SELECT [name] FROM master.dbo.sysdatabases
```

Non-user databases can be excluded with the following `WHERE` clause:

```sql
SELECT [name] FROM master.dbo.sysdatabases WHERE [name] NOT IN ('master', 'tempdb', 'model', 'msdb')
```

> Reporting services databases can also be excluded.
{: .prompt-tip }

This exclusion can also be done on the `dbid` field:

```sql
SELECT [name] FROM master.dbo.sysdatabases WHERE [dbid] > 4
```

> `dbid` between 1 and 4 are system databases, `dbid` between 5 and 6 are usually report server databases.
{: .prompt-info }

### List tables from the current database

Note that there is a lot of ways to achieve this.

```sql
SELECT * FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE = 'BASE TABLE'
```

### Create operator

With [`sp_add_operator`](https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/sp-add-operator-transact-sql?view=sql-server-ver16):

```sql
EXEC msdb.dbo.sp_add_operator @name=N'John Doe', 
    @enabled=1, 
    @weekday_pager_start_time=90000, 
    @weekday_pager_end_time=180000, 
    @saturday_pager_start_time=90000, 
    @saturday_pager_end_time=180000, 
    @sunday_pager_start_time=90000, 
    @sunday_pager_end_time=180000, 
    @pager_days=0, 
    @email_address=N'john.doe@foo.com', 
    @category_name=N'[Uncategorized]'
GO
```

### Add rights to select the operators

The user must exist `msdb` database:

```sql
USE [msdb]
GO
GRANT SELECT ON [dbo].[sysoperators] TO [myUser]
GO
```

### Add rights to execute a specific stored procedure

Ie. with `sp_send_dbmail`:

```sql
USE [msdb]
GO
GRANT EXECUTE ON [dbo].[sp_send_dbmail] TO [myUser]
GO
```

### Mailing

### See database mail configuration

Right click on `Management > Database Mail`, select `Configure Database Mail`.

#### List mail profiles

With the `sysmail_help_profile_sp` procedure:

```sql
EXEC msdb.dbo.sysmail_help_profile_sp;
```

#### Be allowed to send mail

THe best way is to be part of the `DatabaseMailUserRole` role:

```sql
USE [msdb]
GO
EXEC sp_addrolemember N'DatabaseMailUserRole', N'myUser'
GO
-- Then the user is mapped to a profile
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp
    @profile_name = 'myProfile',
    @principal_name = 'myUser',
    @is_default = 1 ;
```

#### Send mail

With the [`msdb.dbo.sp_send_dbmail`](https://docs.microsoft.com/fr-fr/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql?view=sql-server-ver16) procedure:

```sql
EXEC msdb.dbo.sp_send_dbmail
    @profile_name = 'MyProfile',
    @recipients = 'my@email.com',
    @subject = 'subject',
    @body_format = 'HTML',
    @body = 'body'
```

### Scripts collection

[Useful collection](https://docs.dbatools.io/).
