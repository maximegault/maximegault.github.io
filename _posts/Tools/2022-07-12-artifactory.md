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
curl -u <USER>:<TOKEN> -X PUT "https://artifactory.myDomain.com/artifactory/myFolder/destinationFile.txt" -T "pathTo\sourceFile.txt"
```

### With header authentication

With the `-H` for the bearer authorization header:

```bash
curl -H "Authorization: Bearer <TOKEN>" -X PUT "https://artifactory.myDomain.com/artifactory/myFolder/destinationFile.txt" -T "pathTo\sourceFile.txt"
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
curl -u <USER>:<TOKEN> -O "https://artifactory.myDomain.com/artifactory/myFolder/myFile.txt"
```

### With header authentication

With the `-H` for the bearer authorization header:

```bash
curl -H "Authorization: Bearer <TOKEN>" -O "https://artifactory.myDomain.com/artifactory/myFolder/myFile.txt"
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
curl -u <USER>:<TOKEN> -X DELETE "https://artifactory.myDomain.com/artifactory/myFolder-Local/myFile.txt"
```

It should return a `204` code.

### With header authentication

With the `-H` for the bearer authorization header:

```bash
curl -H "Authorization: Bearer <TOKEN>" -X DELETE "https://artifactory.myDomain.com/artifactory/myFolder-Local/myFile.txt"
```
