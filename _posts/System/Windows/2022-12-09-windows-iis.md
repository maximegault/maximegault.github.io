---
title: IIS
date: 2022-12-09 14:00:00 HH:MM:SS +0100
categories: [System, Windows, IIS]
tags: [system, windows, iis]
---

## Generate a configuration change script from IIS Manager GUI

Go to the `Configuration editor`, create a change and click on `Generate script`:

![Script generation from IIS Manager GUI](/assets/img/posts/windows-iis-generate-script.png)
_Script generation from IIS Manager GUI_

## Troubleshooting failed requests using tracing

First be sure that the role `Tracing` is installed:

![Check if the Tracing role is installed](/assets/img/posts/windows-iis-tracing-01.png)
_Check if the Tracing role is installed_

Then go to the web site level, and enable the tracing:

![Go to the web site level](/assets/img/posts/windows-iis-tracing-02.png)
_Go to the web site level_

![Enable Tracing](/assets/img/posts/windows-iis-tracing-03.png)
_Enable Tracing_

Then go to a web application level, and create a tracing rule:

![Go to a web application level](/assets/img/posts/windows-iis-tracing-04.png)
_Go to a web application level_

![Add a Tracing rule](/assets/img/posts/windows-iis-tracing-05.png)
_Add a Tracing rule_

![Setting a Tracing rule, step 1](/assets/img/posts/windows-iis-tracing-06.png)
_Setting a Tracing rule, step 1_

![Setting a Tracing rule, step 2](/assets/img/posts/windows-iis-tracing-07.png)
_Setting a Tracing rule, step 2_

![Setting a Tracing rule, step 3](/assets/img/posts/windows-iis-tracing-08.png)
_Setting a Tracing rule, step 3_

![Tracing rule created](/assets/img/posts/windows-iis-tracing-09.png)
_Tracing rule created_

Now we can have `xml` traces and open them with a browser. Wa have access to a lot of details (ie. a bearer token):

![Trace details, example 1](/assets/img/posts/windows-iis-tracing-10.png)
_Trace details, example 1_

![Trace details, example 2](/assets/img/posts/windows-iis-tracing-11.png)
_Trace details, example 2_

![Trace details, example 3](/assets/img/posts/windows-iis-tracing-12.png)
_Trace details, example 3_
