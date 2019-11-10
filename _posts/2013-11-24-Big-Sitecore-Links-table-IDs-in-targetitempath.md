---
layout: post
title:  "Big Sitecore Links table / IDs in targetitempath"
date:   2013-11-24 11:34:10 +0100
categories: [Sitecore]
---
Do you have these symptoms in your site?

The link table (Usually in the core database) is too big, even bigger than your master or web databases<!--more-->
There are not paths for target item in the link database for your multilist fields, only IDs
Then you´re affected by this problem:

Due to a bug in sitecore, it was storing the value of the property in the (table) field targetitempath, instead of the actual path.

## Here you have different solutions:

1. Contact support and reference ticket 398330 or fix 309475
2. Wait until sitecore release a new version including the fix
3. Fix it yourself.
4. To fix it yourself

- Truncate your link table
- Download this [zip](https://github.com/ClearPeopleLtd/SitecoreLinktableFix/blob/master/Solution/Solution.zip?raw=true) file and copy files to your solution. This file contains a new fieldtypes.config, based on a default version, so if you have done any modification to your fieldtypes.config, you´ll have to modify it manually.
- Rebuild the link tabase.

If you are curious and want to have a look to the code and changes, I’ve created a small solution with the original code from Sitecore and the code introduced by support. You can get it from [here](https://github.com/ClearPeopleLtd/SitecoreLinktableFix)

## My History with this problem

We´re hosting our application in Azure with Azure SQL. By default I limited the databases to 10GB.

One day I found the site was down, due to “reached quota” of the database. I found my core database was 10gb (and maybe bigger if I wouldn’t have it limited).

After contacting support and sending them our database, they found the problem and provide us the usual hotfix to solve the problem.

The fix worked like a charm. Here you have some numbers to show how “big the bug is”

Data before applying the hotfix: 552.894 items = 1.55GB

Data after applying the hotfix: 411.627 items = 0.26GB