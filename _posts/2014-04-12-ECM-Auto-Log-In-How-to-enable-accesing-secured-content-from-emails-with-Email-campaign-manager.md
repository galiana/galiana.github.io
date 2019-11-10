---
layout: post
title:  "ECM Auto Log-In: How to enable accesing secured content from emails with Email campaign manager "
date:   2014-04-12 11:34:10 +0100
categories: [Sitecore, ExM]
---

First of all, keep in mind this is just a "prove of concept" and all the security concerns this kind of solutions could have.

 
It's common to include links to secured areas of your website like "manage your preferences". Usually the user has to be redirected to a log in page before accessing the expected page.
With this module the user will be automatically logged in, making your email integration much more user friendly.

## Current functionality:
- ECM Integration
This module don't add any extra parameter to the links. All the functionality is based on ECM API

- Multi-site support.
Each site can have it's how settings. 
You can specify for which sites the system is listening.

- Double security check
The module can be configured to use the campaign and the automation state as token before logging in a user, reducing the chances of "unexpected" users.

- Link expiration
It will let you decide after how long a link won't log in the users.

- Persistent log in
You can decide it the user will be remembered the next time he access the site.

- Notifications
You can show a notification message to your users to give them any sort of information.

- Pre-configured to use notify.js
Style and text configurable per site.

- Token replacement for message text (same functionality as in the email)

- The Sublayout / rendering  / place holder used to show the message is configurable.

More to come...
## HOW TO USE IT

1. Install the installation [package](https://marketplace.sitecore.net/en/Modules/ECM_AutoLogin.aspx) from the downloads section, as usual. 

2. Create a new item under your root item based on the template: /sitecore/templates/Galiana/Ecm Auto Login/Auto LogIn Options

3. Publish the new items:
/sitecore/layout/Sublayouts/Galiana/
/sitecore/templates/Galiana
Your new item

4. Send a new email an begin playing with it.