---
layout: post
title: "Install Windows Services"
excerpt: "A post to test author overrides using a data file."
tags: []
comments: true
---

## Installing Windows Services

    > [Stackoverflow](http://stackoverflow.com/q/18508628/139637) reference question.
    > **Examples**
    > [Knowledge Base Microsoft](http://support.microsoft.com/kb/251192)
    > [Create, Query, Stop and Delete examples](http://www.herongyang.com/Windows/Service-Create-Delete-Services-with-sc-exe.html)
    > Discussion about parameters: http://stackoverflow.com/questions/3663331/creating-a-service-with-sc-exe-how-to-pass-in-context-parameters

My end solution is aggregation of the results with some clean up.

**GOAL - Create an windows service based on a Console Application**

## How to create a windows service based on a console application.
Mainly the methos OnStart and OnStop need to be implemented, they are part of ServiceBase class.

In Visual Studio, go to new project and pick Windows Service (C# in my case).

Override the OnStart and OnStop method.
Basic [tutorial](http://www.csharp-examples.net/create-windows-service/) that helps to create the basic steps. (it's ugly but quite helpful)

## Install the service

### How to install windows services with PowerShell:
My goal is manage everything with the build server, so I just rely on bash.
#### Create
    New-Service -name cimpltestservice -displayName "Cimpl Test" -binaryPathName "C:\Temp\Service\Release\Etelesolv.EmailService.exe"

    New-Service -name cimplservice-cipweb -displayName "Cimpl Service - Cip Web" -binaryPathName "C:\cimplservices\cip\web\Etelesolv.EmailService.exe"

#### Query
    Get-WmiObject win32_service -filter "name='cimpltestservice'"

#### Delete 
In theory this should work, but in my case it just flaged the service to be removed and I had to restart windows:

    (Get-WmiObject Win32_Service -filter "name='cimplservice-webcip'").Delete()

In other hand this works like a charm:

    .\sc.exe delete cimplservice-webstage

### How to install windows services with cmd (boring):

sc create cimplservice-webstage binPath="C:\cimplservices\cip\web\Etelesolv.TM.SendingInvoices.Console.exe" DisplayName="Cimpl Service - Web Stage" start=demand

