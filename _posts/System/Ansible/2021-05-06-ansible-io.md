---
title: Ansible IO
date: 2021-05-06 16:00:00 HH:MM:SS +0100
categories: [System, Linux, Ansible, YAML]
tags: [system, linux, ansible, yaml, tips]
---

### Check if a file exists and creates it if it doesn't

With the [stat module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/stat_module.html):

```yaml
- ansible.builtin.stat: 
    path: pathToFile
  register: file_exists
```

Can be used like that in a test, ie. create it if does not exist with the [file module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html):

```yaml
- ansible.builtin.file:
    path: pathToFile
    state: touch
  when: file_exists.stat.exists == false
```

The file can also be directly created when writing into if with the [lineinfile](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html) or [blockinfile](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/blockinfile_module.html) modules with the `create` parameter set to `yes`. See next paragraphs.

### Check if a line exists in a file

With the [lineinfile module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html):

```yaml
- name: Check if the_line exists in the_file
  ansible.builtin.lineinfile:
    path: the_file
    regexp: '^the_line'
    state: absent
  # See https://docs.ansible.com/ansible/latest/user_guide/playbooks_checkmode.html
  check_mode: yes
  # See https://docs.ansible.com/ansible/latest/user_guide/playbooks_error_handling.html
  # It tricks this task to be considered as never changing, so this playbook stays indempotent
  changed_when: false
  register: the_line_existence_test
```

Directly create the file with the `create` parameter set to `yes`:

```yaml
- name: Check if the_line exists in the_file
  ansible.builtin.lineinfile:
    path: the_file
    line: 'the_line'
    create: yes
```

### Write a block in a file

With the [blockinfile module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/blockinfile_module.html) (here the file can also be directly created with the `create` parameter set to `yes`):

```yaml
- name: Writing lines in the the_file
  ansible.builtin.blockinfile:
    path: the_file
    marker: "# {mark} ANSIBLE MANAGED BLOCK"
    block: |
      line1
      line2
    backup: yes
```
