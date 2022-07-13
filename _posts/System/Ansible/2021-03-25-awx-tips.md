---
title: AWX tips
date: 2021-03-25 14:00:00 HH:MM:SS +0200
categories: [System, Linux, Ansible, AWX, tower]
tags: [system, linux, ansible, awx, tower, tips]
---

Here we consider that we have two hosts, `host_1` and `host_2`.

### Theory

A few things to know:

* a project is a link to a repository that can contains playbooks, inventories, variables files, Python scripts, etc.
* a job template is linked with a project, a given playbook (from this project), a given inventory and credentials. Variables can be set there too
* a template workflow is a set of job templates

### Access to a variable defined in a job template

Additional variables defined in the job template:

```yaml
---
runtime_var: "Hi I'm a runtime variable from a job template"
# This is used to be access dynamically to a variable that belongs to a unique host
host_1:
  message: "FOO"
host_2:
  message: "BAR"
```

Access to these variables through the playbook:
<!-- {% raw %} -->
```yaml
---
- name: "Dummy playbook"
  # ...
  tasks:      
    - name: Display runtime variable
      debug:
        msg: '{{ runtime_var }}'
    - name: Display specific dynamic host variable from job template
      debug:
        msg: "My message {{ vars[ansible_hostname].message }}"
```
<!-- {% endraw %} -->
### Access to a variable defined in a host

Host variable:

```yaml
---
host_var: "BAZ"
```

Access to this variable through the playbook:
<!-- {% raw %} -->
```yaml
---
- name: "Dummy playbook"
  # ...
  tasks:      
    - name: Display host variable
      debug:
        msg: "Defined with the host: {{ hostvars[inventory_hostname].host_var }}"
```
<!-- {% endraw %} -->
### Access to a variable defined in a inventory

Inventory variable:

```yaml
---
inventory_var: "BOO"
```

Access to this variable through the playbook:
<!-- {% raw %} -->
```yaml
---
- name: "Dummy playbook"
  # ...
  tasks:      
    - name: Display inventory variable
      debug:
        msg: "Defined with the inventory: {{ inventory_var }}"
```
<!-- {% endraw %} -->
### Access to a variables file through vars_files

AWX copies the playbook files into its own structure, so a relative path must be used:

```yaml
---
vars_files: ./vars/myFile.yml
```

### Dynamic inventory

A "dynamic" inventory comes from a repository, thus it can be a yaml file or the result of a Python script execution.

To use it :

* create a project mapped to the repository
* create an inventory:
  * create a "Sourced from a Project" source in this inventory to use the target file/script
* the the inventory can be used in another job template

#### Inventory from script

If an inventory comes from a script, ie. Python or bash, this script must be marked as executable on the repository side. To do this ([source](https://git-scm.com/docs/git-update-index#Documentation/git-update-index.txt---chmod-x)):

```bash
git update-index --chmod=+x path/to/file
```

Variables can be defined in the inventory's source, ie.:

```yaml
---
inventory_source_var: "Foo"
```

This variable is set as an environment variable and can be used directly in the target script (see in the target langage's documentation how to use environment variables).
