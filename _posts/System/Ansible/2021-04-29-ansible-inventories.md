---
title: Ansible inventories
date: 2021-04-29 14:30:00 HH:MM:SS +0200
categories: [System, Linux, Ansible, YAML, inventory]
tags: [system, linux, ansible, yaml, tips, inventory, inventories]
---

### Inventory in YAML format

Example:

```yaml
---
all:
  children:
    group_1:
      hosts:
        customAlias:
          ansible_host: "realHostName.myDomain.com" 
          var_1: "Foo" 
        customAlias_2:
          ansible_host: 192.168.1.2 
          var_2: 12345 
    group_2:
      hosts:
        "realHostName2.myDomain.com"
```

### Inventory in INI format

[Excellent article on Ansible inventories - INI format only](https://www.digitalocean.com/community/tutorials/how-to-set-up-ansible-inventories)

### Check an inventory syntax

Examples with the one given in YAML. To have a list:

```bash
ansible-inventory -i inventory --list
```

Output:

```json
{
    "_meta": {
        "hostvars": {
            "customAlias": {
                "ansible_host": "realHostName.myDomain.com",
                "var_1": "Foo"
            },
            "customAlias_2": {
                "ansible_host": "192.168.1.2",
                "var_2": 12345
            },
            "realHostName2.myDomain.com": {}
        }
    },
    "all": {
        "children": [
            "group_1",
            "group_2",
            "ungrouped"
        ]
    },
    "group_1": {
        "hosts": [
            "customAlias",
            "customAlias_2"
        ]
    },
    "group_2": {
        "hosts": [
            "realHostName2.myDomain.com"
        ]
    },
    "ungrouped": {}
}
```

To have a simplier graph:

```bash
ansible-inventory -i inventory --graph
```

Output:

```text
@all:
  |--@group_1:
  |  |--customAlias
  |  |--customAlias_2
  |--@group_2:
  |  |--realHostName2.myDomain.com
  |--@ungrouped:
```

## Inventories to use in local

### In INI format

Most basic:

```ini
localhost ansible_connection=local
```

### In YAML format

The same one in YAML:

```yaml
all:
  children:
    ungrouped:
      hosts:
        localhost:
          ansible_connection: local
```

### Convert INI inventory in YAML one

The YAML seems more easy to me to manage complex structures:

```bash
ansible-inventory -i inventory_ini -y --list > inventory_yaml
```

### Inventory from Python script

A dynamic inventory can be built from a Python script. This script has to:

* answer to a call with the `--list` argument that will be used in background by Ansible
* output an inventory in a JSON format

Two useful links:

* [Ansible official documentation](https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html)
* [Excellent blog article by Jeff Geerling](https://www.jeffgeerling.com/blog/creating-custom-dynamic-inventories-ansible)

Simple example, built with the previous links (mostly Jeff Geerling's one), that returns the previous inventory in a JSON format (note that the static JSON inventory can be build with the `ansible-inventory -i inventory --list` command). Note that:

* it also sends a mail with all the environment variables (useful in an AWX context)
* we have removed the `ungrouped` part (in order to avoid to have an `ungrouped` group in AWX)

```python
#!/usr/bin/env python
from __future__ import print_function
from email.message import EmailMessage

DOCUMENTATION = r'''
Dynamic inventory (from the Ansible side) that returns a static JSON inventory.
'''

import os
import sys
import argparse
import smtplib
import datetime
import time

try:
    import json
except ImportError:
    import simplejson as json

class DynamicInventory(object):

    def __init__(self):
        self.inventory = {}
        self.read_cli_args()

        # Called with `--list`.
        if self.args.list:
            self.inventory = self.example_inventory()
        # Called with `--host [hostname]`.
        elif self.args.host:
            # Not implemented, since we return _meta info `--list`.
            self.inventory = self.empty_inventory()
        # If no groups or vars are present, return an empty inventory.
        else:
            self.inventory = self.empty_inventory()

        # Build the environment variables list to put then in a mail
        output = ""
        for a in os.environ:
            output +=('Var: {} Value: {} \r\n'.format(a, os.getenv(a)))

        self.send_mail(output)
        print(json.dumps(self.inventory))
        
    def send_mail(self, body):
        address = 'john.doe@myDomain.com'
        msg = EmailMessage()
        msg.set_content(body)
        msg['Subject'] = 'Environment variables'
        msg['From'] = address
        msg['To'] = address

        # Send the message via our own SMTP server.
        s = smtplib.SMTP('smtp.myDomain.com', 25)
        s.send_message(msg)
        s.quit()

    # Example inventory for testing.
    def example_inventory(self):
        return {
            "_meta": {
                "hostvars": {
                    "customAlias": {
                        "ansible_host": "realHostName.myDomain.com",
                        "var_1": "Foo"
                    },
                    "customAlias_2": {
                        "ansible_host": "192.168.1.2",
                        "var_2": 12345
                    },
                    "realHostName2.myDomain.com": {}
                }
            },
            "all": {
                "children": [
                    "group_1",
                    "group_2"
                ]
            },
            "group_1": {
                "hosts": [
                    "customAlias",
                    "customAlias_2"
                ]
            },
            "group_2": {
                "hosts": [
                    "realHostName2.myDomain.com"
                ]
            }
        }

    # Empty inventory for testing.
    def empty_inventory(self):
        return {'_meta': {'hostvars': {}}}

    # Read the command line args passed to the script.
    def read_cli_args(self):
        parser = argparse.ArgumentParser()
        parser.add_argument('--list', action = 'store_true')
        parser.add_argument('--host', action = 'store')
        self.args = parser.parse_args()

# Get the inventory.
DynamicInventory()
```
