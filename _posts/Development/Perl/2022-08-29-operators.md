---
title: Perl operators
date: 2022-08-29 09:00:00 HH:MM:SS +0200
categories: [Development, Perl]
tags: [development, perl, operators]
---

### Comparison operators

| Type | Numeric | String |
| --- | --- | --- |
| Equality | `==` | `eq`
| Difference | `!=` | `ne`
| Difference | `<=>` | `cmp`
| Less than | `<` | `lt`
| Greater than | `>` | `gt`
| Less than or equal | `<=` | `le`
| Greater than or equal | `>=` | `ge`

> `<=>` and `cmp` return `1` if left value is greater than right value, `0` if left value equals right value and `-1` if left value is lower than right value.
{: .prompt-info }

### Logical operators

| Type | Operator |
| --- | --- |
| and | `&&`
| or | `||`
