---
title: Execute as
date: 2020-10-29 08:30:00 HH:MM:SS +0100
categories: [Development, T-SQL]
tags: [development, t-sql, sqlserver, database]
---

### Execute a query as a different user/login

Useful to debug rights problems:

```sql
EXECUTE AS LOGIN = 'myLogin';  
-- Display current execution context and verify the execution context is now myLogin  
SELECT SUSER_NAME(), USER_NAME();  
-- Login1 sets the execution context to myUser.  
EXECUTE AS USER = 'myUser';  
-- Display current execution context and verify the execution context is now myUser   
SELECT SUSER_NAME(), USER_NAME();  
```

See:

* [full documentation](https://docs.microsoft.com/fr-fr/sql/t-sql/statements/execute-as-transact-sql?view=sql-server-ver15)
