---
title: Ansible config
date: 2021-05-31 10:00:00 HH:MM:SS +0100
categories: [System, Linux, Ansible, YAML]
tags: [system, linux, ansible, yaml, tips]
---

### See Ansible configuration

To see Ansible configuration, use the `ansible-config` utility, ie.:

```bash
ansible-config list
```

Infos that you can find:

```text
DEFAULT_MODULE_PATH:
  default: ~/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
  description: Colon separated paths in which Ansible will search for Modules.
  env:
  - {name: ANSIBLE_LIBRARY}
  ini:
  - {key: library, section: defaults}
  name: Modules Path
  type: pathspec
```

[Full documentation](https://docs.ansible.com/ansible/latest/cli/ansible-config.html#ansible-config)

### Change parameters in Ansible configuration

The configuration files are:

* `/etc/ansible/ansible.cfg`: config file, used if present
* `~/.ansible.cfg`: user config file, overrides the default config if present

A `.cfg` file can also be present at the playbook level

#### Add a custom path

Example for modules: ie. edit the `/etc/ansible/ansible.cfg` file, at the beginning everything is commented:

```ini
[defaults]

# some basic default values...

#inventory      = /etc/ansible/hosts
#library        = /usr/share/my_modules/
```

Just add a custom path (use a `:` to separate values):

```ini
[defaults]

# some basic default values...

#inventory      = /etc/ansible/hosts
library        = /usr/share/my_modules/:~user/ansible_custom/library
```

If we execute a playbook in verbose mode (`-vvv`), we can see:

```bash
ansible-playbook -i hosts.yaml playbook.yml -vvv
```

Output:

```text
ansible-playbook 2.7.7
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/usr/share/my_modules', '/home/user/ansible_custom/library']
```

For filters, etc., it's here:

```ini
# set plugin path directories here, separate with colons
#action_plugins     = /usr/share/ansible/plugins/action
#cache_plugins      = /usr/share/ansible/plugins/cache
#callback_plugins   = /usr/share/ansible/plugins/callback
#connection_plugins = /usr/share/ansible/plugins/connection
#lookup_plugins     = /usr/share/ansible/plugins/lookup
#inventory_plugins  = /usr/share/ansible/plugins/inventory
#vars_plugins       = /usr/share/ansible/plugins/vars
filter_plugins     = /usr/share/ansible/plugins/filter:~user/ansible_custom/filter
#test_plugins       = /usr/share/ansible/plugins/test
#terminal_plugins   = /usr/share/ansible/plugins/terminal
#strategy_plugins   = /usr/share/ansible/plugins/strategy
```
