---
layout: post
title:  "SiteCore Reports"
date:   2012-06-05 11:34:10 +0100
categories: [Sitecore]
---
We can find a good, free and Open Source reporting framework, ready to install on the SiteCore Shared Source Modules, called Advanced System Reporter.<!--more-->

[Project website](http://trac.sitecore.net/AdvancedSystemReporter)
[Download link](http://trac.sitecore.net/AdvancedSystemReporter/browser/Trunk/data/packages/Advanced%20System%20Reporter-1.4.2.zip?format=raw)

Copying from the project documentation, ASR is a little framework which allows creating many types of reports in very short time. This module provides this framework together with many useful example reports (23):

- Get the Alias of an Item
- Audit users Actions
- Find broken links
- Find broken link in publishable items
- Expired media items
- Item History
- Items with invalid names
- Items with security for an account
- Links
- Locked Items
- Login errors
- Logged In users
- Multiple Versions
- My owned Items
- Not recently modified
- Orphaned media assets
- Owners of items
- Workflow history
- Items recently modified
- Search with regular Expression (regex)
- Renderings
- Stale workflow items (Items in a workflow status for a long time)
- Validation Errors

Let's explain how this module work, configuring a scheduled task, to send one email with the modified items during last week:

## Install the module
You'll find a SiteCore ready to use module at the download link, so you just need to run the installation Wizard (SiteCore Desktop), upload the zip, and follow the wizard to the end. Keep in mind, that according to the project documentation, this module needs Sitecore CMS V6.0, v.6.4.1, v.6.5

## Setup and test
By default, you'll find a link to the application under main menu ==> All Applications ==> Reporter.
![Sitecore reports](/static/img/posts/Sitecorereports.png "Sitecore reports")

This tool, let us configure, run, export and save previously generated reports. We're going to use the report "Recently modified", so we click on the Open button (Upper left corner), and select the appropriated item.

![Sitecore reports2](/static/img/posts/Sitecorereports2.png "Sitecore reports2")

We've to choose the root item, usually "content\home", and since we want to get the modified items. That's all. Now just press "Run" on the window top menu bar, and "voilà", we get our list of modified items. If you're and admin, it is possible to export all that valuable information as csv, xls (an html file, but with Excel extension), or xml, clicking on the button "Export". If you want to allow to all users to export the reports, you must install [this](http://trac.sitecore.net/AdvancedSystemReporter/browser/Trunk/data/packages/Advanced%20System%20Reporter-Download-1.0.zip?format=raw) package.

Once our report is ready, we only have to save it, so it can be run, with our selecting the parameter values manually. But what happened behind the scene? By default, all the preconfigured items are stored under "/sitecore/system/Modules/ASR/Reports". Now, the application have saved our "report" as a child of the "/sitecore/system/Modules/ASR/Reports/Recently Modified", using the template "/sitecore/templates/System/ASR/Saved Report". So, what we've saved is not a report, are a "report parameter set", associated to a report.

## Set up the task to be done by the scheduler
Now, we need a specific item to be executed by the scheduler. The ASR, come with a template to simplify this task, under "/sitecore/templates/System/ASR/Report Email Task".

So, let's select the commands foder under "/sitecore/system/Tasks", right click and "Insert à Insert from template", on the next windows, we've to choose the mentioned item "/sitecore/templates/System/ASR/Report Email Task". We must fill the fields under the "report Email section".

## Active: If the report must be executed
To, Subject and text: The properties of the email to be send.
Send if empty. By default, the application won't send the email, if after running the report, it return 0 items. If you want to send the email anyway, mark this option.
Report. Here we can choose all the reports we need on the same email. Notice, here will only opera "Saved Reports". It make sense, because you can't run reports without parameters.
Save, and the task is ready.

## Schedule the task
The specific ASR steps are done, now, we only need to set up and default SiteCore Schedule Item. Select the "Schedules" folder under "/sitecore/system/Tasks/Schedules", right click à Insert à Insert Schedule.

On the Command field, choose the command created in the prior step.

On the Schedule field, we set up when to run the command. This field is a bit estrange: Let's explain it with an example: the value: ||127|23:59:59 Each "|", is a separator; the first value, blank in this case, is the first day to run the task in yyyyMMdd format. The second is the last day. Again blank in this case, it means, the task will be executed undefinitively. The third one is the day of the week, 127 means every day. Why? 1=Sunday, 2=Monday, 4=Tuesday, 8=Wednesday, 16=Thursday, 32=Friday, and 64=Saturday, so everyday =1+2+4+8+16+32+64=127, and fourth parameter is the minimum interval between invocations. 23:59:59, means one a day. Obvisuly, the report will not be executed at exactly the same time every day, but is the best approach we've. If we try to user 24:00:00, the system will translate it 00:00:00, so the task will be executed every second. You can read more about scheduled task on SiteCore [here](http://www.sitecore.net/Community/Technical-Blogs/John-West-Sitecore-Blog/Posts/2010/11/All-About-Sitecore-Scheduling-Agents-and-Tasks.aspx)

That's all, SiteCore will run the report and send the result via email, once a day.

## Advanced configuration
At this point, you could have realized that our report will send us the mail with the modified items since some specific day, we select in the 3, but what if we want the items modified "yesterday" or last week? Well, it is possible too.

We've to modify the "Saved report", so let's go to "/sitecore/system/Modules/ASR/Reports/Recently Modified", and click on the item created on step 3. There's only on field, with all the parameters of the report separated by the character "^". Find the parameter AgeISO and replace the value for one of these variables:

- $sc_lastyear
- $sc_lastweek
- $sc_lastmonth
- $sc_yesterday
- $sc_today
- $sc_now

## A framework, Why?
The application come with 23 preconfigured reports, but we can create as many records as we want, adding filters, parameters, scanners, viewers, etc., so we have the tools to create, easly our own custom report. How to do this? well, maybe on the next post.

## Some reports are not working, why?!
First, check your logs, and look for something like "WARN ASR: cannot assign value to property…". If you find this error, you're lucky. These errors are easy to solve.

The module is using SiteCore utilities to set the attributes of the different configuration items as filters, scanners, etc. Those elements have the assembly, class and attributes, configured on SiteCore fields for this purpose.

If you open the filter /sitecore/system/Modules/ASR/Configuration/Filters/Audit, you'll see the field "Attributes", with this value: "verb={Verb}|user={User}" This means that the code is going to try to set the attribute "verb" and the attribute "user", of one object of type "AuditFilter" (namespace: ASR.Reports.Logs), but if you check the code (Downloading the code, or with some Decompiler as "[Telerik just decompile](http://www.telerik.com/products/decompiler.aspx)"), you'll see that this type, have two properties: "Verb" and "User". So, as you can expect, the method "Sitecore.Reflection.ReflectionUtil.SetProperty", is not able to find the property "verb" and "user".

The solution: edit the attributes field in the sitecore item, change the first letter of the parameters to uppercase and save. You'll report will run properly now.