---
layout: post
title:  "Using real sizes (px) in the rich text editor (RTE) of sitecore"
date:   2013-09-19 11:34:10 +0100
categories: [Sitecore]
---
The default drop down for the font size in the RTE, have a list of relative numbers, which will be translated into px, in the html.<!--more-->

![Default font size drop down](/static/img/posts/Defaultfontsizedropdown.png "Default font size drop down")

It´s possible to change this drop down by another with pixels:

Switch to the core database
Navigate to /sitecore/system/Settings/Html Editor Profiles
There is one folder per profile, the default is "Rich Text full"
In this case, Open the tool bar 3, and select "Font Size"
Update the field "clic", change the "FontSize" default value, by "RealFontSize"
That´s it. Now you´ll know the real font size you´re applying to your text.

![Real font size drop down](/static/img/posts/Realfontsizedropdown.png "Real font size drop down")

