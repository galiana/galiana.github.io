---
layout: post
title:  "Invalid lincese after moving VM from one data center to another"
date:   2017-05-31 11:34:10 +0100
categories: [Azure]
---

Some months ago we had to move our servers from Germany to North Europe and recently windows is asking us to provide a valid product key.

Running the command slmgr.vbs /dlv we could see that the registered KMS machine name was kms.core.cloudapp.de

Apparently, for Nort Europe (at least) the server name must be kms.core.windows.net

To change the licensing server, we have to run the following line in an elevated porwshell console:

iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms kms.core.windows.net:1688”
Now we only have to wait a few minute and request a new license with the command:

slmgr.vbs /ato
That's it, as simple as that. Now our servers have a valid license again.

P.S. We are not sure if the cause of the issue is the change of datacenter, but for sure, using the right license server fixed the issue.