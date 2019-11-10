---
layout: post
title:  "Sitecore Azure Error Startup task ended with error 532462766"
date:   2014-03-29 11:34:10 +0100
categories: [Sitecore, Azure]
---
How to fix the error 532462766<!--more-->

One of the settings necessary to run sitecore on azure as PaaS is a valid connection to a "Azure storage".

Sitecore Azure takes care of this, and creates a new one for you (With an horrible name) and set it up for you on your instance, but "sometimes", you may rename it, change the key, or simply, during different test, creating and deleting things, at some point this connexion gets broken, and when you create / upgrade your Instance, you receive the error  532462766. This simply means, The App fabric is not able to  access the storage provided on the settings. How to fix it? Easy, open the item of your instance "/sitecore/system/Settings/Azure/Environments/your envronment/your instance", and edit the field "service definition", search for your blob, table and queu settings and replace the values for the ones in the manager.