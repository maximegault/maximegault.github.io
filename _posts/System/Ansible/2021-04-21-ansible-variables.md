---
title: Ansible variables
date: 2021-04-21 08:00:00 HH:MM:SS +0100
categories: [System, Linux, Ansible, YAML]
tags: [system, linux, ansible, yaml, tips]
---

### Access a variable thanks to its name

Example playbook:

```yaml
---
- name: "Amazing playbook"
  vars_files: 
    - /vars/myVars.yml
```

The variables file `/vars/myVars.yml`, example with a variable named with a host name:

```yaml
---
myHost_1:
    unique_variable: "Foo"
myHost_2:
    unique_variable: "Bar"
```

Dynamic access in a playbook through the host name with `vars` dictionary:
<!-- {% raw %} -->
```yaml
# Looping on hosts myHost_1 and myHost_2, it will contain "Foo" then "Bar"
myDynamicVariable: "{{ vars[inventory_hostname_short].unique_variable }}" 
```
<!-- {% endraw %} -->
### Access to a dynamic vars_files at the playbook level

Warning the facts are not accessible yet:
<!-- {% raw %} -->
```yaml
  vars_files: "./vars/{{ inventory_hostname_short }}.yml"
```
<!-- {% endraw %} -->
### Set a fact as `undefined`

Just leave it empty:

```yaml
vars:
  undefined_fact_trick:
```
