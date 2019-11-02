---
layout: post
title:  "Adding publish item to the contextual menu of Sitecore"
date:   2017-06-29 15:34:10 +0100
categories: [sitecore]
image: publish context.png
---

How many times have you right-click on an item to publish it to remember that this option is not available? How annoying it is for you, having to go to "Publish" tab, click on publish item, etc... I got fed up while working on LawNow Upgrade and decided to add it. It's a 5 min task and saves "hours" of frustration...

Change to the core database
Go to: /sitecore/content/Applications/Content Editor/Context Menues/Default
Duplicate the item Insert (Or any other of template "/sitecore/templates/System/Menus/Menu item". I recommend copying, as this template is not in the "insert options".
Rename the new item as "Publish"
Change the field display name to "Publish"
Set the Icon field to "Office/16x16/publish.png"
Set the Message field to: item:publish(id=$Target)
Change the Item icon to "Office/16x16/publish.png"
To be fair, I have to give the credits to Benjamin Vangansewinkel and his post, who gave me the idea. I just added the icons to make it look nicer...

That's it, now you should get something like this:
![Publish on context menu](https://raw.githubusercontent.com/galliana/galiana.github.io/master/static/img/_posts/publish context.png  "Publish on context menu")

That's it, now run to show it to your experienced Sitecore users and watch their faces of joy!