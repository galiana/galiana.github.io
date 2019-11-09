---
layout: post
title:  "Sitecore consumes all the available memory and restarts"
date:   2017-05-28 11:34:10 +0100
categories: [Sitecore, SQL Azure]
---

Recently, one of my biggest instances went mad and decided to consume all the available memory on the server, to finally die and restart. No deployment, nor code changes, no editions, nothing...
<!--more-->
I then noticed that some hours before we had a full site republished, which apparently was completed without errors.

As we had some issues before with the event queue and related tables I decided to try a shortcut. I truncated all these tables, restarted the application and voila! Sitecore went back to normal.

I always have my magic queries on hand to clean the queues:

'''sql

delete FROM [History] where Created < DATEADD(HOUR, -12, GETDATE());

delete FROM [PublishQueue] where Date < DATEADD(HOUR, -12, GETDATE()); 

delete FROM [EventQueue] where [Created] < DATEADD(HOUR, -4, GETDATE());

'''

But, as usually these problems are related to too many events queued, I decided to go for the shortest way and run:

'''sql

truncate table [History];

truncate table [PublishQueue]; 

truncate table [EventQueue];

'''
 

To note that I was using a low, standard tier of SQL Azure, so the performance of some queries is not really good.

Disclaimer: Don't run SQL scripts against a production environment without full understanding of what you are doing and the related risks