---
title: Ansible loops
date: 2021-04-29 14:30:00 HH:MM:SS +0200
categories: [System, Linux, Ansible, YAML, loop]
tags: [system, linux, ansible, yaml, tips, loop, loops]
---

## Loops

### Simple loop

With the given example list:

```yaml
my_list:
  - foo
  - bar
  - baz
```

Just use the `loop` keyword:
<!-- {% raw %} -->
```yaml
- ansible.builtin.debug:
    msg: item
  loop: "{{ my_list }}"
```
<!-- {% endraw %} -->
### Get loop's extended infos

Just use the `loop_control` keyword.

#### Change loop variable name
<!-- {% raw %} -->
```yaml
- ansible.builtin.debug:
    msg: list_item
  loop: "{{ my_list }}"
  loop_control:
    loop_var: list_item
```
<!-- {% endraw %} -->
#### Get loop's indexes

Use the `loop_control` keyword and set `extended` to true to get much more info.
<!-- {% raw %} -->
```yaml
- include_tasks: taskThatUseLoopItem.yml
  loop: "{{ my_list }}"
  vars:
    # Current loop index (O indexed)
    loop_index_zero: "{{ ansible_loop.index0 }}"
    # Current loop index (1 indexed)
    loop_index_one: "{{ ansible_loop.index }}"
  loop_control:
    loop_var: list_item
    extended: yes
```
<!-- {% endraw %} -->
[Ansible loops reference](https://docs.ansible.com/ansible/latest/user_guide/playbooks_loops.html)
