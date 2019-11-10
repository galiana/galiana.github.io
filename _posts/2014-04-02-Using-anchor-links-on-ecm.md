---
layout: post
title:  "Using anchor links on ecm"
date:   2014-04-02 11:34:10 +0100
categories: [Sitecore]
---

If you try to add a link to an anchor somewhere else in your email, you'll realise ecm could be replacing your link with the  base url.
<!--more-->
In theory, ECM shouldn't replace links where the url starts with "#", but ECM 2.1 revision 140214 (Update 3) it's breaking my links with this replacements.

ECM is replacing links via searching for the text "href=" in your body. So one solution is as easy as change your mark-up to "href =". This single space will make your link invisible to CMS, keeping your links to your anchor "safe".

This is a sample link:

<a href = '#anchor' target="_self">link text</a>