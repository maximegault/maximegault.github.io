---
title: T-SQL development
date: 2022-07-08 09:00:00 HH:MM:SS +0200
categories: [Development, T-SQL]
tags: [development, t-sql, sqlserver, database]
---

### Get an ID from a label

Ie. with the given table:

```sql
CREATE TABLE [Dummy](
    [Id] [numeric](18, 0) IDENTITY(1,1) NOT NULL,
    [Label] [nvarchar](32) NOT NULL
```

Get the ID in a single variable with the exact label:

```sql
DECLARE @tempId AS NUMERIC(18,0)
-- Parentheses are mandatory
SET @tempId = (SELECT [Id] FROM [Dummy] WHERE [Label] = 'ExactLabel')
```

### Get single values from a select that return a unique line

With previous table example:

```sql
DECLARE @tempId AS NUMERIC(18,0)
DECLARE @tempLabel AS NVARCHAR(32)
-- Parentheses are mandatory
SELECT 
    @tempId = [Id], 
    @tempLabel = [Label] 
FROM
    [Dummy] 
WHERE
    [Label] = 'ExactLabel'
```
