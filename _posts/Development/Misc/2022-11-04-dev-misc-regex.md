---
title: AWS CLI
date: 2022-11-04 10:00:00 HH:MM:SS +0100
categories: [Development, Regex]
tags: [development, regex]
---

### Match a line that does not contain a word

Example with "`it`" word:

```text
^((?!it).)*$
```

Sources:

* [stackoverflow](https://stackoverflow.com/questions/406230/regular-expression-to-match-a-line-that-doesnt-contain-a-word)
* [regex101](https://regex101.com/r/d95dAZ/1)
