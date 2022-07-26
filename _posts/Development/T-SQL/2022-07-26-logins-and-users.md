---
title: Logins and users
date: 2022-07-26 09:00:00 HH:MM:SS +0200
categories: [Development, T-SQL]
tags: [development, t-sql, sqlserver, database, login, user]
---

### Create login

```sql
CREATE LOGIN [myLogin] WITH PASSWORD=N'myPassword', DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[us_english], CHECK_EXPIRATION=ON, CHECK_POLICY=ON
GO
```

### Create user

```sql
CREATE USER [myUser] FOR LOGIN [myLogin]
```

### Transfer logins and passwords between instances

With [`sp_help_revlogin`](https://docs.microsoft.com/en-US/troubleshoot/sql/security/transfer-logins-passwords-between-instances):

```sql
EXEC sp_help_revlogin
GO
```

It transfers all logins and users, needed rights, etc.

### See given database "orphan" users (not linked to a user - perhaps deleted)

With [`sp_change_users_login`](https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/sp-change-users-login-transact-sql?view=sql-server-ver16):

```sql
USE [myDatabase]
EXEC sp_change_users_login 'Report';
```

Orphan users can be linked back with an alter

```sql
USE [myDatabase]
ALTER USER [myOrphanUser] WITH LOGIN = [myExistingLogin];
```
