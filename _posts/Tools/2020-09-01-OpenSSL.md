---
title: OpenSSL
date: 2020-09-01 09:00:00 HH:MM:SS +0200
categories: [Tools]
tags: [tools, openssl, certificate]
---

[OpenSSL binaries](https://wiki.openssl.org/index.php/Binaries)

### Split a .p12 file into three files

* private key (.key file)
* client certificate (.crt file)
* certification authority certificate (.crt file)

```batch
openssl.exe pkcs12 -in C:\input\certificate.p12 -nocerts -out C:\outputs\privateKey.key
openssl.exe pkcs12 -in C:\input\certificate.p12 -clcerts -nokeys -out C:\outputs\clientCertificate.crt
openssl.exe pkcs12 -in C:\input\certificate.p12 -cacerts -nokeys -out C:\outputs\caCertificate.crt
```

### Convert a base64 encoded pkcs12 certificate into a .p12 file

Consider the pkcs12 is contained in a `pkcs12.txt` file:

```batch
openssl.exe base64 -d -a -in pkcs12.txt -out irn-myCertificate.p12
```

### Get all .p12 data

It will display client certificate, CA certificate, private key, etc.:

```batch
openssl.exe pkcs12 -info -in C:\input\certificate.p12 -nodes
```

[Source](https://www.ssl.com/how-to/export-certificates-private-key-from-pkcs12-file-with-openssl/)

### Get public key from .p12

Result is:

```batch
openssl.exe pkcs12 -in C:\input\certificate.p12 -clcerts -nokeys -out C:\outputs\certificate.pem
```

```text
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
```

### Get private key from .p12

Result is:

```batch
openssl.exe pkcs12 -in C:\input\certificate.p12 -clcerts -nodes -out C:\outputs\certificate.key
```

```text
-----BEGIN PRIVATE KEY-----
...
-----END PRIVATE KEY-----
```
