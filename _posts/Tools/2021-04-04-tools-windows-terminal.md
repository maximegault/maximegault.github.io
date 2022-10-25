---
title: Windows terminal
date: 2021-04-04 15:00:00 HH:MM:SS +0100
categories: [Tools]
tags: [tools, windows, terminal]
---

### Change the default opened terminal

Locate the `defaultProfile` line and set its value with the needed terminal GUID:

```json
{ "defaultProfile": "{b4be9505-f320-4a0f-b648-3e51f03cd66c}" }
```

```json
{ 
    "list": [
        {
            "guid": "{93eb43cd-0a92-4b6d-99dc-b3fb64e1536c}",
            "name": "Windows PowerShell",
            "commandline": "powershell.exe",
            "hidden": false
        },
        {
            "guid": "{b4be9505-f320-4a0f-b648-3e51f03cd66c}",
            "name": "My custom powershell installation",
            "commandline": "powershell.exe",
            "hidden": false,
            "startingDirectory": "C:\\MyCustomDirectory",
            "icon": "💣"
        }
    ]
}
```

Hint: [get a GUID](https://duckduckgo.com/?q=guid&t=ironbrowser&ia=answer)

### Change the distribution and the user to open the WSL with

In the WSL entry, change the `commandline` parameter value:

* `-d`: the distribution's name
* `-u`: the user's name

```json
{ "commandline": "wsl -d distributionName -u userName" }
```
