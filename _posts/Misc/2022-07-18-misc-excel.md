---
title: Excel
date: 2022-07-18 09:00:00 HH:MM:SS +0200
categories: [Excel, Misc]
tags: [excel, misc]
---

### New line in CONCAT or other function

Just use the character that belongs to the number `10` with `CHAR` function, ie. with `CONCAT`:

```vb
CONCAT("NEW";CHAR(10);"LINE");
```

Output:

```text
NEW
LINE
```

### Functions EN/FR translations

| Function | EN | FR |
| --- | --- | --- |
| Strings concatenation | [CONCAT](https://support.microsoft.com/en-us/office/concat-function-9b1a9a3f-94ff-41af-9736-694cbd6b4ca2) | [CONCAT](https://support.microsoft.com/fr-fr/office/concat-concat-fonction-9b1a9a3f-94ff-41af-9736-694cbd6b4ca2)
| Get a character specified by a number | [CHAR](https://support.microsoft.com/en-us/office/char-function-bbd249c8-b36e-4a91-8017-1c133f9b837a) | [CAR](https://support.microsoft.com/fr-fr/office/fonction-car-bbd249c8-b36e-4a91-8017-1c133f9b837a)

[Functions names translated](https://www.perfectxl.com/excel-glossary/what-is-excel-function/translations-french-english/)
