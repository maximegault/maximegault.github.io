---
title: Ansible tips
date: 2021-04-16 08:00:00 HH:MM:SS +0100
categories: [System, Linux, Ansible, YAML]
tags: [system, linux, ansible, yaml, tips]
---

### Ad-hoc mode

Used to test a module without having a playbook. Ie. with a ping on all hosts contained in the `inventory` file (`-m` is the argument for the module name):

```bash
ansible -i inventory all -m ping
```

Same thing with setup:

```bash
ansible -i inventory all -m ansible.builtin.setup
```

[Ad-hoc reference](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html)

### Format a pattern

Example with the input values:

```yaml
server_pattern: "ldap://%s:%s"
server_uri: "myServer.com"
server_port: 389
```
<!-- {% raw %} -->
```yaml
final_server_uri: "{{ server_pattern | format(server_uri, server_port) }}"
```
<!-- {% endraw %} -->
It will give:

```text
ldap://myServer.com:389
```

### Pad left an int with leading zeros

With two variables, one string and one int:

```yaml
var_int: 4
var_string: "5"
```

Two leading zeros examples with [format](https://jinja.palletsprojects.com/en/2.9.x/templates/#format) template:
<!-- {% raw %} -->
```yaml
- name: Pads left an int value
  ansible.builtin.set_fact:
    final_var_1: "{{ '%02d' | format(var_int) }}"
    # Here we first cast the var_string into an int in order to add the two variables
    final_var_2: "{{ '%02d' | format(var_string | int + var_int) }}"
```
<!-- {% endraw %} -->
Final values:

```text
final_var_1 = "04"
final_var_2 = "09"
```

### Classic facts

On a target host called `myMachine.myDomain.com`:

* `ansible_hostname` = `myMachine`
* `ansible_fqdn` = `myMachine.myDomain.com`

### Magic variables

On a target host called `myMachine.myDomain.com`:

* `inventory_hostname_short` = `myMachine`
* `inventory_hostname` = `myMachine.myDomain.com`

[Magic variables vs. facts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_vars_facts.html#)

### Special variables

Magic variables + classic facts = special variables

[Special variables reference](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html)

### Check if a packages is installed (ie. `httpd` for Apache)
<!-- {% raw %} -->
```yaml
- name: Gettings package facts
  package_facts:
  changed_when: false

- name: Creating a fact for Apache installation status
  ansible.builtin.set_fact:
    apache_is_installed: "{{ ('httpd' in ansible_facts.packages) | ternary(1, 0) }}"
  changed_when: false
```
<!-- {% endraw %} -->
[Package facts reference](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_facts_module.html)