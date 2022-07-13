---
title: Markdown
date: 2021-04-29 8:30:00 HH:MM:SS +0100
categories: [Markdown, Misc, Tips]
tags: [markdown, misc, tips]
---

### A backtick into backticks

Two bacticks to escape a single backtick:

```markdown
`` foo` `` produces foo`
```

If there is a backtick at the beginning at the end or at the start, a space must be added:

```markdown
``foo` `` produces foo`
`` `foo` `` produces `foo`
`` `foo`` produces `foo
```

Each additional backtick must be escaped with an additional backtick (so two ``"`"`` to escape one ``"`"``, three ``"`"`` to escape two ``"`"``, etc.):

```markdown
``` ``foo`` ``` produces ``foo``
```` ```foo``` ```` produces ```foo```
```

Sources:

* [Meta Stack Exchange](https://meta.stackexchange.com/a/82722)
* [Daring fireball](https://daringfireball.net/projects/markdown/syntax)

### Markwown reference

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
