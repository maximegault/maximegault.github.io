---
title: AWS CLI
date: 2022-06-23 09:00:00 HH:MM:SS +0200
categories: [Development, AWS, CLI]
tags: [development, aws, cli]
---

### AWS CLI

#### First settings

[AWS CLI reference](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html)

Installation on Windows with `myTenant` tenant:

* Get [the MSI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* Optional, desactivate the proxy for the S3 host:

```batch
setx /m NO_PROXY bucket.s3storage.myTenant.fr
```

* Perform the first configuration steps with the `aws configure` command:

```batch
aws configure
```

It will ask for:

```batch
AWS Acces Key ID =
AWS Secret Access Key =
Default region name =
Default output format =
```

* Then we can test the connection ie. listing the repository `myRepo` on the `myTenant` tenant content with the `s3` command and the `ls` argument:

```batch
aws --endpoint-url https://bucket.s3storage.myTenant.fr s3 ls s3://myRepo/
```

* If there is a certificate problem, just put the appropriate `.pem` file in a directory, ie. `C:\Certificates` and launch the following command:

```batch
setx /m AWS_CA_BUNDLE "C:\Certificates\myCertificate.pem"
```

#### Commands reference

[Commands reference](https://docs.aws.amazon.com/cli/latest/index.html)

#### S3

Use the `s3` command ([S3 commands reference](https://docs.aws.amazon.com/cli/latest/reference/s3/index.html)).

#### List a repo

With the `ls` argument ([ls reference](https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html)):

```batch
aws --endpoint-url https://bucket.s3storage.myTenant.fr s3 ls s3://myRepo/
```

#### Copy a file

With the `cp` argument ([cp reference](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html)):

```batch
aws --endpoint-url https://bucket.s3storage.myTenant.fr s3 cp source destination
```

It's the same command to get a file `from` the repo, ie.:

```batch
aws --endpoint-url https://bucket.s3storage.myTenant.fr s3 cp s3://myRepo/myFile C:\destinationFolder
```

And for putting a file `on` the repo:

```batch
aws --endpoint-url https://bucket.s3storage.myTenant.fr s3 cp C:\sourceFolder\myFile s3://myRepo/myFile
```

#### Delete a file on the repo

With the `rm` argument ([rm reference](https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html)):

```batch
aws --endpoint-url https://bucket.s3storage.myTenant.fr s3 rm s3://myRepo/myFile
```
