---
title: HashiCorp Vault
date: 2022-04-14 09:00:00 HH:MM:SS +0100
categories: [Markdown, Misc, HashiCorp, Vault]
tags: [markdown, misc, hashicorp, vault]
---

### Request secret with cURL

Here with a cURL specfiic version (that can read from Windows certificates store):

```bash
curl -x "<PROXY>" --request POST -k --cert CERT_THUMBPRINT --data "{\"name\": \"VAULT_ROLE\"}" VAULT_URL -i -verbose --trace-ascii .\trace-ascii.txt
```

With:

* `PROXY`: target proxy
* `CERT_THUMBPRINT`: full certificate path and thumbprint, ie. `LocalMachine\My\12345612345764CE4A71D440E20FAC5E5123456`
* `VAULT_ROLE`: the role granted to use Vault
* `VAULT_URL`: Vault `login` endpoint, ie. `https://myVault.com/v1/auth/cert/login`
