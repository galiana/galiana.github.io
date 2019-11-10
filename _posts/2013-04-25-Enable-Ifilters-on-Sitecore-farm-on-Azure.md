---
layout: post
title:  "Enable Ifilters on a Sitecore farm on Azure"
date:   2013-04-25 11:34:10 +0100
categories: [Sitecore, Azure]
---
The scope of this post is just about how to enable / install the Microsoft Ifilters pack; How to use them in your code is a different matter, but i can tell you using IFilters will be OOTB with Sitecore 7.<!--more-->

In order to get started, i'll assume you're going to do it directly in your "deployer" instance of Sitecore. Yes, the one with the Sitecore Azure Module 2.0.

Go to: yourrootfolder\Website\App_Data\AzureOverrideFiles and create a folder called "ExternalComponent" (Or any other name)
Download the IFilter pack from here. Remember to choose the 64 bit version and save it into theprevious folder
Open the file yourrootfolder\Website\App_Data\AzureOverrideFiles\StartUp.cmd
Add a new line under:"%windir%\system32\inetsrv\appcmd set config -section:applicationPools -applicationPoolDefaults.processModel.idleTimeout:00:00:00"
Add this text:
ECHO  Installing IFilters
start %RoleRoot%\approot\ExternalComponents\FilterPack64bit.exe /passive /norestart /quiet
Save the file.
Upgrade files on your farm.
That's it, after a few minuts your IFilters  will be waiting for you!
The startup.cmd file is executed by the appfrabric, on when calling to the RoleStart method, by default Sitecore added some lines to modify rights and disable the apppool to "fall aslret",. What we've done is simply, execute the installation of the package. As simple as that.

This worked for us, if you found any problem or a better solution... please, let us know.
