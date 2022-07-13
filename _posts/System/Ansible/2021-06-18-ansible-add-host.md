---
title: Ansible add host
date: 2021-06-18 10:00:00 HH:MM:SS +0100
categories: [System, Linux, Ansible, YAML]
tags: [system, linux, ansible, yaml, tips, add, host]
---

### Add an host at runtime

The `add_host` module is useful to dynamically add an host to the current playbook execution. Classic usage would be:

* a task creates an host, ie. a VM
* the next task has to be performed on the newly created VM

A less usual usage, ie. from an AWX side, would be to have a job templated linked with an empty inventory and a playbook where all the hosts will be dynamically added.

Here a simple example that will use the following variables file to add hosts dynamically (this variables file could also be loaded dynamically depending on a variable set on the AWX side):

```yaml
---
my_hosts:
  - ansible_hostname_short: 'host_1'
    ansible_hostname: 'host_1.myDomain.com'
    instances:
      - index: 1
        port: "11389"
        secure_port: "11636"
        alias: "D01"
  - ansible_hostname_short: 'host_2'
    ansible_hostname: 's4855ars.myDomain.com'
```

Here an example playbook called from AWX, where a `awx_defined_group` variable is defined:
<!-- {% raw %} -->
```yaml
---
- name: 'First part: we just dynamically add the hosts'
  # localhost must be kept here
  hosts: localhost
  connection: local
  gather_facts: no
  # Loading a host file at runtime
  vars_files: "./vars/hosts_{{ awx_defined_group }}.yml"
  tasks:          
    # Adding all the hosts
    - name: Add hosts 
      add_host:
        hostname: '{{ host_item.ansible_hostname_short }}'
        ansible_hostname_short: '{{ host_item.ansible_hostname_short }}'
        ansible_hostname: '{{ host_item.ansible_hostname }}'
        # Important in our example: a group must be set
        groupname: '{{ awx_defined_group }}'
      with_items: "{{ my_hosts }}"
      loop_control:
          loop_var: host_item

# Here a new part with another "tasks" keyword where we also set a new value for the "hosts" keyword 
- name: 'Second part: using new hosts'
  # We only use the knewly added group
  hosts: '{{ awx_defined_group }}'
  tasks:
    # A simple ping to be sure that we can call our new groups
    - ansible.builtin.ping:
```
<!-- {% endraw %} -->
[Module's documentation](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/add_host_module.html)
