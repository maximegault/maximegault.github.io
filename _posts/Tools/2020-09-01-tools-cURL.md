---
title: cURL
date: 2020-09-01 10:00:00 HH:MM:SS +0200
categories: [Tools]
tags: [tools, curl, certificate]
---

[cURL man page](https://curl.haxx.se/docs/manpage.html)

### Work with a client certificate

#### Use a .p12 file as a client certificate

```bash
cURL ... --cert-type P12 --cert myCertificate.p12
cURL ... --cert-type P12 --cert myCertificate.p12:myPassword
```

#### Use a certificate from the Windows store

A cURL that works with WinSSL must be used!

```bash
cURL ... --cert storeLocation\storeName\thumbprint
```

Example:

```bash
cURL ... --cert LocalMachine\My\969d0cacd0f76ec68e10feda75a28b58a1d54968
```

#### Use separate client certificate, certification authority certificate and private key files

See [OpenSSL](../OpenSSL/) for details on the different files

```console
cURL ... --cert myClientCertificate.crt --key myPrivateKey.key --cacert myCaCert.crt 
```
