---
title: Generic informations
date: 2020-10-29 08:30:00 HH:MM:SS +0100
categories: [Development, T-SQL]
tags: [development, t-sql, sqlserver, database, column]
---

### List tables from the current database

Note that there is a lot of ways to achieve this.

```sql
SELECT * FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE = 'BASE TABLE'
```

### Create user

```sql
CREATE USER [myUser] FOR LOGIN [myLogin]
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