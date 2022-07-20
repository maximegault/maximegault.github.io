---
title: JFrog Artifactory
date: 2022-07-12 15:00:00 HH:MM:SS +0200
categories: [Tools, Artifactory, cURL]
tags: [tools, artifactory, curl]
---

## Main documentation

[Here](https://www.jfrog.com/confluence/display/JFROG/Artifactory+REST+API)

## Deploy a file

### With basic authentication

With the:

* `-u` for the user
* `-X PUT` argument
* `-T` argument

```bash
curl -u <USER>:<PASSWORD> -X PUT "https://artifactory.myDomain.com/artifactory/myFolder/destinationFile.txt" -T "pathTo\sourceFile.txt"
```

Or:

```bash
curl -u <USER>:<ARTIFACTORY_TOKEN> -X PUT "https://artifactory.myDomain.com/artifactory/myFolder/destinationFile.txt" -T "pathTo\sourceFile.txt"
```

### With header authentication

With the `-H` for:

* Either the bearer `Authorization` header:

```bash
curl -H "Authorization: Bearer <BEARER_TOKEN>" -X PUT "https://artifactory.myDomain.com/artifactory/myFolder/destinationFile.txt" -T "pathTo\sourceFile.txt"
```

* r the JFrog Artifactory `X-JFrog-Art-Api` header:

```bash
curl -H "X-JFrog-Art-Api: <ARTIFACTORY_TOKEN>" -X PUT "https://artifactory.myDomain.com/artifactory/myFolder/destinationFile.txt" -T "pathTo\sourceFile.txt"
```

## Download a file

### With basic authentication

With the:

* `-u` for the user
* `-O` argument

```bash
curl -u <USER>:<PASSWORD> -O "https://artifactory.myDomain.com/artifactory/myFolder/myFile.txt"
```

Or:

```bash
curl -u <USER>:<ARTIFACTORY_TOKEN> -O "https://artifactory.myDomain.com/artifactory/myFolder/myFile.txt"
```

### With header authentication

With the `-H` for:

* Either the bearer `Authorization` header:

```bash
curl -H "Authorization: Bearer <BEARER_TOKEN>" -O "https://artifactory.myDomain.com/artifactory/myFolder/myFile.txt"
```

* Or the JFrog Artifactory `X-JFrog-Art-Api` header:

```bash
curl -H "X-JFrog-Art-Api: <ARTIFACTORY_TOKEN>" -O "https://artifactory.myDomain.com/artifactory/myFolder/myFile.txt"
```

### Add a checksum

With the `X-Checksum-<ALGORITHM>` header, ie. `X-Checksum-MD5` or `X-Checksum-SHA1`:

```bash
curl -H "X-JFrog-Art-Api: <ARTIFACTORY_TOKEN>" -X PUT "https://artifactory.myDomain.com/artifactory/myFolder/destinationFile.txt" -T "pathTo\sourceFile.txt" -H "X-Checksum-MD5:F6367DBFA4932A601A753D1E4DD381D1"  -H "X-Checksum-SHA1:2F11CBD40205FF68205894AD8A02A5EB0C94DEA0"
```

## Delete a file

Remark: use the `-Local` version of the folder.

### With basic authentication

Then use:

* `-u` for the user
* `-X DELETE` argument

```bash
curl -u <USER>:<PASSWORD> -X DELETE "https://artifactory.myDomain.com/artifactory/myFolder-Local/myFile.txt"
```

Or:

```bash
curl -u <USER>:<ARTIFACTORY_TOKEN> -X DELETE "https://artifactory.myDomain.com/artifactory/myFolder-Local/myFile.txt"
```

It should return a `204` code.

### With header authentication

With the `-H` for:

* Either the bearer `Authorization` header:

```bash
curl -H "Authorization: Bearer <BEARER_TOKEN>" -X DELETE "https://artifactory.myDomain.com/artifactory/myFolder-Local/myFile.txt"
```

* Or the JFrog Artifactory `X-JFrog-Art-Api` header:

```bash
curl -H "X-JFrog-Art-Api: <ARTIFACTORY_TOKEN>" -X DELETE "https://artifactory.myDomain.com/artifactory/myFolder-Local/myFile.txt"
```
