---
title: Ansible lists and dictionaries
date: 2021-05-06 16:00:00 HH:MM:SS +0100
categories: [System, Linux, Ansible, YAML, Lists, Dictionaries]
tags: [system, linux, ansible, yaml, tips, lists, dictionaries]
---

### Add a key/value pair to an existing dictionary

Another dictionary is needed, and the use of `combine`:
<!-- {% raw %} -->
```yaml
- name: First dictionary
  ansible.builtin.set_fact:
    my_dictionary:
      property_1: "Foo"
      property_2: "Bar"
      
- name: Temp dummy fact to add a new key value
  ansible.builtin.set_fact:
    dummy_dictionary:
      property_3: "Baz"
  
- name: Adding a new fact to the current dirsrv instance one
  ansible.builtin.set_fact:
    my_dictionary: "{{ my_dictionary | combine(dummy_dictionary) }}"
```
<!-- {% endraw %} -->
`my_dictionary` contains:

```json
{
    "my_dictionary": {
        "property_1": "Foo",
        "property_2": "Bar",
        "property_3": "Baz"
    }
}
```

### Convert a simple list into a dictionary

With the input list:

```yaml
strings:
  - 'firstString'
  - 'secondString'
```

To convert it into a dictionary, here with the list item as key and `false` as value:
<!-- {% raw %} -->
```yaml
- ansible.builtin.set_fact:
    strings_with_state_into_dictionary: "{{ strings_with_state_into_dictionary | default({}) | combine ({ string_item : false }) }}"
  loop: "{{ strings }}"
  loop_control:
    loop_var: string_item
```
<!-- {% endraw %} -->
`default({})` initializes `strings_with_state_into_dictionary` with an empty dictionary, `combine` fills the dictionary with the `key : value` syntax.

### Example of complex data manipulation

Here we need to check for an input mounts list:

* if each mount exist
* if each mount is mounted on a separate device

With this given input list:

```yaml
input_mounts:
  - '/'
  - '/my_directory'
  - '/my_directory/my_user'
```

Getting all the mounts:
<!-- {% raw %} -->
```yaml
- name: Getting all existing mounts
  ansible.builtin.set_fact:
    existing_mounts: "{{ ansible_mounts | map(attribute='mount') | list }}"
  changed_when: false
```
<!-- {% endraw %} -->
First step, `ansible_mounts` (Ansible fact), will produce something like that:

```json
{
    "block_available": 503381,
    "block_size": 4096,
    "block_total": 521728,
    "block_used": 18347,
    "device": "/dev/mapper/vg00-lvroot",
    "fstype": "xfs",
    "inode_available": 1047462,
    "inode_total": 1048576,
    "inode_used": 1114,
    "mount": "/",
    "options": "rw,relatime,attr2,inode64,noquota",
    "size_available": 2061848576,
    "size_total": 2136997888,
    "uuid": "98760513-d53a-4801-8f37-7fb813ffdf24"
},
{
    "block_available": 629910,
    "block_size": 4096,
    "block_total": 1046016,
    "block_used": 416106,
    "device": "/dev/mapper/vg00-lvusr",
    "fstype": "xfs",
    "inode_available": 2054141,
    "inode_total": 2097152,
    "inode_used": 43011,
    "mount": "/usr",
    "options": "rw,nodev,relatime,attr2,inode64,noquota",
    "size_available": 2580111360,
    "size_total": 4284481536,
    "uuid": "1345becf-f2ca-49db-a7dc-8103c65bde67"
}
```

Second step, `map(attribute='mount')`, will "map" every object into a new one by selecting only the attribute `mount`. It would give something like that (not outputed with a `debug` task):

```json
{
    "mount": "/"
},
{
    "mount": "/usr"
}
```

Then the `list` will turn these objects into a unique list with the unique attribute (so `mount`) values:

```json
{
    "msg": [
        "/",
        "/usr"
    ]
}
```

Now we can check if every input mount is defined:
<!-- {% raw %} -->
```yaml
- name: Checking if all the needed mounts exist
  ansible.builtin.assert:
    that: item in existing_mounts
    fail_msg: "Mount {{ item }} does not exist"
  with_items: "{{ input_mounts }}"
```

To check if each mount is mounted on a separate device, we'll need to group Ansible mounts by device, using `{{ ansible_mounts | groupby('device') }}`, in order to have a mounts list per device (there can be several mounts on the same device). With the previous sample, it would give:
<!-- {% endraw %} -->
```json
{
    "msg": [
        [
            "/dev/mapper/vg00-lvroot",
            [
                {
                "mount": "/",
                "device": "/dev/mapper/vg00-lvroot",
                "fstype": "xfs",
                "options": "rw,relatime,attr2,inode64,noquota",
                "size_total": 2136997888,
                "size_available": 2061864960,
                "block_size": 4096,
                "block_total": 521728,
                "block_available": 503385,
                "block_used": 18343,
                "inode_total": 1048576,
                "inode_available": 1047461,
                "inode_used": 1115,
                "uuid": "98760513-d53a-4801-8f37-7fb813ffdf24"
                }
            ]
        ],
        [
            "/dev/mapper/vg00-lvusr",
            [
                {
                "mount": "/usr",
                "device": "/dev/mapper/vg00-lvusr",
                "fstype": "xfs",
                "options": "rw,nodev,relatime,attr2,inode64,noquota",
                "size_total": 4284481536,
                "size_available": 2580959232,
                "block_size": 4096,
                "block_total": 1046016,
                "block_available": 630117,
                "block_used": 415899,
                "inode_total": 2097152,
                "inode_available": 2054141,
                "inode_used": 43011,
                "uuid": "1345becf-f2ca-49db-a7dc-8103c65bde67"
                }
            ]
        ]
    ]
}
```

The final test:

* we loop on the mounts list grouped by device
* on each item of this list, so one loop per device:
  * we declare a variable `temp_mounts` that contains the list of the mounts of the current device
  * we intersect this variable with the input mounts. If the intersection's length is greater than one, it would mean that several input mounts are on the same device

<!-- {% raw %} -->
```yaml
- name: Checking if the needed mounts are all mounted on separated devices
  ansible.builtin.assert:
    # temp_mounts belong to an unique device, so we intersect its list with the input one: if more than one match it means that there is more than one input mount on the same device
    that: temp_mounts | intersect(input_mounts) | length <= 1
    # In the loop, item.0 is the dictionary's current item's key
    fail_msg: "Multiple mounts {{ temp_mounts }} on device {{ item.0 }}"
  # We group the existing mounts by device, and build a list of mounts that belong to the given device (ie. { dev1: [mnt1, mnt2], dev2: [mnt3] }). We loop on it
  loop: "{{ ansible_mounts | groupby('device') }}"
  vars:
    # In the loop, item.1 is the dictionary's current item's value (so the mounts list): thus in each loop we retrieve the mounts that belong to the current device,   ie.    [mnt1, mnt2] for dev1
    temp_mounts: "{{ item.1 | map(attribute='mount') | list }}"
```
<!-- {% endraw %} -->