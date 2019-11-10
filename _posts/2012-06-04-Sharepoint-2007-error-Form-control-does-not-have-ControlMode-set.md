---
layout: post
title:  "Sharepoint 2007 error: Form control does not have ControlMode set"
date:   2012-06-04 11:34:10 +0100
categories: [Sharepoint]
---
One of the most common tasks when branding a SharePoint installation is to generate custom master page to be used in your freshly branded site<!--more-->. As a front end developer, in an attempt to simplify the masterpage, and further management and maintenance of the master page, you tend to (or at least, I tend to) try and remove all the unnecessary code and place-holders you’re not going to use. In a SharePoint master page this can be hundreds of lines!

However, if you start removing items from the Master Page you will soon learn that you cannot simply remove everything you won’t need, because SharePoint needs some of them! This becomes more complex because when you test your new master page, you get a nice generic error page, without any description of the problem! To make the error page more meaningful you need to modify the custom error configuration of your test web site to be able to find the error.

To do this Open the file web.config at “c:\Inetpub\wwwroot\wss\VirtualDirectories\yousitecollection”, and modify the line <customErrorsmode="RemoteOnly" />and set the mode to Off. Now you’ll get the typical asp.net error page with a clear message complaining about some missing placeholder.

So whats next? You add this placeholder again. After repeating these steps, you’ll find a page like this, explaining the minimal place holders your master page must have and some tricks to have a working SharePoint Site. So, what is the first lesson when modifying the Master Page? Do it the SharePoint way! This means don’t remove things you don’t know or don’t need, just hide them!

This is all relatively simple, and there are lots of resources talking about this, so it’s “easy” to create an apparently working master page. My nightmare began when I tested it with a “News archive” page, of a “News Site”, suddenly the error: Server error in “/” application Form control does not have ControlMode set.

So what did I do? After rechecking all of the possible placeholders, and my Master Page with the default master page and other minimal master pages I found on internet, I decided to trace the application to try to find the problem.

After a bit of work I finally found the line that was generating the error  was the “<SharePoint:FormField runat="server" id="TitleField" FieldName="Title" />”in the file C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\SiteTemplates\SPSNHOME.

This is great to now, but is not of much use as I know I shouldn’t modify the SharePoint installation pages, so… back to my master page. After many hours moving, removing, copying and comparing, I found the issue. So take this as lesson number 2: The order of the place holders in the master page is important too!!!

In my master page, the place holder for the “PlaceHolderPageTitleInTitleArea” was before the delegate control for the publishing Console (<SharePoint:DelegateControl runat="server" ControlId="PublishingConsole"/>). I just moved the PlaceHolderPageTitleInTitleArea placeholder after the console and problem solved!

After digging a bit more, I got to this conclusion: The formField (Microsoft.SharePoint.WebControls.FormField) control needs the ControlMode to know how to render the value of the field. You could set it in the same control, but then, it’d render always the same way, independently if the page is in edit mode, view mode, etc. and that’s the way SharePoint is using this practice. Let the publishing console set the ControlMode.

So when working with SharePoint Master Pages you have the following 3 constraints:

You should not modify the SharePoint installation files.
You can’t use the control SharePoint:FormField before the publishing console (Or something else has to set the ControlMode).
You don’t know where SharePoint is using this control.
So this has led me to the following two conclusions:

When planning for a new Master Page, adapt your design to insert the publishing console as soon as possible.
Move any placeholder that you’re not using explicitly to the end of the form, inside a hidden panel.