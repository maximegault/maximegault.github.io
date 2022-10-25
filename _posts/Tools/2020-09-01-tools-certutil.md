---
title: certutil
date: 2020-09-01 09:00:00 HH:MM:SS +0200
categories: [Tools]
tags: [tools, certutil, certificate]
---

[Official documentation](https://docs.microsoft.com/fr-fr/windows-server/administration/windows-commands/certutil)

### List certificates from given store name (default store location is local machine)

* The -store option belongs to store name
* The -user option enables to list the "current user" location instead of the "local machine" one

Useful to list certificates without mmc (or certmgr.msc) or to know, for example, if a certificate's private key is exportable or not:

```bash
certutil -store my
```

Output example:

```text
================ Certificate 1 ================
Serial Number: 000abcf000000000000f
Issuer: CN=Foo, DC=Bar, DC=Baz
 NotBefore: 10/9/2020 11:35 AM
 NotAfter: 10/8/2020 11:35 AM
Subject: CN=Toto
Non-root Certificate
Template: 1.3.6.1.4.1.311.21.8.1276197.7416986.11292289.4105614.6793517.173.6633
251.14043945
Cert Hash(sha1): b4 b8 d3 89 b4 67 38 90 82 4d 33 d4 ea fd 48 d7 0f 2e 0d d8
  Key Container = 15bd7cfb-834d-4fc7-1ab00abc49c6ec3
  Provider = Microsoft Base Smart Card Crypto Provider
Private key is NOT exportable
Encryption test passed
```

### Convert a base64 encoded pkcs12 certificate into a .p12 file

Consider the pkcs12 is contained in a `pkcs12.txt` file:

```bash
certutil -decode pkcs12.txt irn-myCertificate.p12
```
