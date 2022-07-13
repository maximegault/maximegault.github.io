---
title: Ansible vault
date: 2021-04-16 08:00:00 HH:MM:SS +0100
categories: [System, Linux, Ansible, vault, awx, tower]
tags: [system, linux, ansible, vault, awx, tower]
---

### Create an encrypted secret in Ansible vault

First steps:

* we have to chose an alias for an encryption key, ie. `myEncryptionKey`
* create an encryption key, ie. a 60 characters upper-case, lower-case and digits like `hvtl6v0ZAbOF2U7hBdHJMBtqTw3KOJLMMXG3l2TiDU0JYwQun7ly1J7U5fdt`

On a machine that has Ansible installed:

* in the `~/ansible` directory, create a file that will contain the encryption key called `~/ansible/.vault_alias`, ie. `~/ansible/.vault_myEncryptionKey`
* copy the encryption key in this file

To use it in AWX:

* create a credential of type `Vault`: in the field `Vault Password` copy the encryption key, in the `Vault Identifier` field the chosen alias, ie. `myEncryptionKey`
* in the template that has to decrypt a secret, add this credential

To encrypt a secret, go back to the machine that has Ansible installed:

* create an `~/ansible.cfg` file that contains the mapping encryptionKeyAlias@encryptionKeyFile, ie. `myEncryptionKey@~/ansible/.vault_myEncryptionKey`:

    ```ini
    # Entries encryptionKeyAlias@encryptionKeyFile has to be separated by comas
    [defaults]
    vault_identity_list = myEncryptionKey@~/ansible/.vault_myEncryptionKey
    ```

* create an `~/encrypt.sh` script that will help to encrypt a secret and create a ready-to-use Ansible variable:

    ```bash
    #/bin/bash
    read -p "Ansible variable name:" ansible_variable_name
    echo
    read -s -p "Secret to encrypt:" secret_to_encrypt
    echo
    read -p "Vault encryption key:" encryption_key_id
    echo
    ansible-vault encrypt_string --encrypt-vault-id $encryption_key_id $secret_to_encrypt --name $ansible_variable_name
    ```

    Execute this script:

    ```bash
    Ansible variable name:test_variable
    Secret to encrypt:
    Vault encryption key:myEncryptionKey
    ```

    It will output:

    ```text
    test_variable: !vault |
          $ANSIBLE_VAULT;1.2;AES256;myEncryptionKey
          38383061353766373834393736653366623932303532626234316337336238613032636466666165
          3636313632643561336262386337383637643436313866630a336338383531316435613266346236
          35376633383334653932623739336333313237626135353735326666386130663765633961313037
          3138633763323538340a643034336433313233613565323835613339333931303463623831653632
          3736
    Encryption successful
    ```

    This variable can be used in the playbook called by the template.
