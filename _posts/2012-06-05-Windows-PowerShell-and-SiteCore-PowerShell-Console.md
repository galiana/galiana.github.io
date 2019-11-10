---
layout: post
title:  "Windows PowerShell and SiteCore PowerShell Console"
date:   2012-06-05 11:34:10 +0100
categories: [Sitecore, PowerShell]
---
The Windows PowerShell is as task-based command-line shell and scripting language (We'll, not everybody agree: [Windows power shell is not a scripting language](https://redmondmag.com/articles/2010/05/01/windows-powershell-is-not-a-scripting-language.aspx)). There are two main differences with common "shells":<!--more-->

Is built on the top of the .Net framework. The use of the framework is not limited to internal use only, in fact, as a developer, (or as a "scripter" you can manipulate real .net objects, and the results, parameters, etc. are objects too, when usually others shell use strings.
It is based on providers to access different data stores. A data store can be the file system, windows registry, databases, WMI, etc... It means you can access and manipulate them using the same cmdlets (pronounced command-lets)
There are lots of blogs and documents about Windows PowerShell on the need, but here you have some useful links for newbies:

[Wikipedia](https://en.wikipedia.org/wiki/Windows_PowerShell): Better than expected

[The Microsoft documentation](https://technet.microsoft.com/en-us/library/bb978526.aspx)

## So, what's this module?
This module gives us two main things:
- The SiteCore powershell provider. That let us access sitecore items, using standard powershell cmdlets (commands)
- A Sitecore application simulating a PowerShell system console, to run our scripts inside Sitecore Desktop.

## Installing the module
The module can be downloaded from the sitecore [trac](http://trac.sitecore.net/SitecorePowershellConsole) as an [installable package](https://bit.ly/scpsc14), so you only need to follow the usual SiteCore steps to install it. If you don't know how, you can find them [here](http://sdn.sitecore.net/articles/administration/installing%20modules%20and%20packages.aspx).

Once the package is installed you'll fin a link to the console in the Desktop menu.

## Security Issues
As we could expect, this tool will do some low level operations that our application pool's user may don't have access to.

This tool use two files allocated in "AppDomainAppPath\Console\Assets" to set up the default view of some SiteCore types, but by default, the power shell environment is configured to don't allow the access to this file, so will see some security warning on your console. Reading "PowerShell's [Security Guiding Principles](https://blogs.msdn.com/b/powershell/archive/2008/09/30/powershell-s-security-guiding-principles.aspx)" you'll learn it deeply, but, fur our porpoise, you must know that the default execution policy is "restricted", it means only typed commands can be executed or in other words, any script file can be executed. So we need to change it, to allow the load of these two files. The process is easy, as you can read [here](http://www.windowsecurity.com/articles/PowerShell-Security.html), you just need to change the Execution policy to "RemoteSigned" (Run all local script and Signed external scripts) with the command "–setexecutionpolicy remotesigned". But if you execute this command you'll see that your application pool user doesn't have enough rights to do this (You are not running your application as an admin, isn't?). My first option was to run this command as an administrator on the default Windows powershell console, but after executing successfully the command, and checking the "get-executionpolicy" command, I keep having the same warnings, so I had to change the identity of the application pool, run the script on the Sitecore PowerShell console" and undo the changes on the app pool. I don't like this solution, due to for a short time, you're running your website with full access to the computer, so any idea is welcome.

## Executing scripts
We have two option to run scripts:
- Executing scripts on the console
This is the easiest way, just open the console, and type or paste you cmdlets on the bottom of the screen and press CTRL+INTRO. The result will be displayed above.
- Executing script as scheduled tasks
This's a great feature, that let you scheduled that great script you've generated and tested on the console. I could explain how to do it, but it won't be better than the [module's author](http://blog.najmanowicz.com/2011/11/29/powershell-driven-sitecore-scheduled-tasks/). But as a resume, you only have to create a script item under "core:/sitecore/content/Applications/PowerShell Console/Scripts", and create a schedule item, using the command "/sitecore/system/Tasks/Commands/PowerShellScriptCommand" created during the installation, and your Script item. Really easy!

## Generating scripts
Now is time to begin playing with it. Maybe the first thing you should do is have a quick reading of some tutorial. I've found some good tutortials at [http://www.powershellpro.com](http://www.powershellpro.com/) like:

- [How to us cmdlets](http://www.powershellpro.com/powershell-tutorial-introduction/tutorial-powershell-cmdlet/)
- [Managing variables](http://www.powershellpro.com/powershell-tutorial-introduction/variables-arrays-hashes/)
- [Conditional logic](http://www.powershellpro.com/powershell-tutorial-introduction/powershell-tutorial-conditional-logic/)
- [Loops](http://www.powershellpro.com/powershell-tutorial-introduction/logic-using-loops/)
Another good idea could be try [Power GUI](http://www.powergui.org/), o good, free IDE for Power shell scripts, with syntax highlight, "intellisense", and a large etc.

## Modifications
On my first tests, I was not able to use the get-item command, finally decided to download the code and debug the module and I found a null object reference. So I modified the code to check the object, before using it.

On the file Trunk\Shell\Provider\PsSitecoreItemProvider.cs

Line 296:

Before: if (dic["Language"].IsSet 
After: if (dic!= null && dic["Language"].IsSet) 
Line 302:

Before: if (dic["Version"].IsSet) 
After: if (dic != null && dic["Version"].IsSet) 
I made another modification to get some messages:

On the file Trunk\Shell\Host\ScriptingHostUserInterface.cs, line 38:

Before: var lastline = Output[Output.Count - 1]; 
After: var lastline = Output[Output.Count > 0 ? Output.Count - 1 : 0]; 

## Sample scripts
Most of the samples, have been taken from the module author's website:

[http://blog.najmanowicz.com/](http://blog.najmanowicz.com/)

- Get item from master database
get-item master:\content\home

- Get item from web database
get-item web:\content\home

- Get children of an item
get-childitem web:\content\home

- Get all the children of an item (recursive)
get-childitem–recurse web:\content\home

- Choose with properties display of an item
Get-Item master:\content\Home | Select-Object DisplayName, TemplateName

See: [http://technet.microsoft.com/en-us/library/dd315291.aspx](http://technet.microsoft.com/en-us/library/dd315291.aspx)

- Show output as a table
get-childitem-recurse master:\\content | format-table Name, "__Updated By", TemplateName

See: [http://technet.microsoft.com/en-us/library/dd315255.aspx](http://technet.microsoft.com/en-us/library/dd315255.aspx)

- Order items
Get-ChildItem master:\content\home-recurse | Sort-ObjectId-Descending | select-object name, TemplateName

See: [http://technet.microsoft.com/en-us/library/dd347688.aspx](http://technet.microsoft.com/en-us/library/dd347688.aspx)

- Find all the properties and methods of and item
Get-Item master:\content\home | Get-Member

See: [http://technet.microsoft.com/en-us/library/ee176854.aspx](http://technet.microsoft.com/en-us/library/ee176854.aspx)

- Set the current item
cd master:\content\home

Remember the scope of this command is the execution of the current script

- Get the current item
Get-item .

- Get the list of Sitecore caches and status
Get-cache

- Get a list of archives on every database
Get-archive

- Get a list of databases
Get-database (database name)

- Get a user specific user
get-user sitecore\admin

- Get a list of indexes
get-index (index name)

- Get a list on search indexes
get-searchindex

- Publish an item
get-item master:\content\home | publish-item 
Optional parameters of publis-item 
Path
Id
-recurse
Targets
Languages
Publishmode
Currentpathinfo
Restart the current application
Restart-application 
Set the PowerShell window properties
Options

-persist
ForegroundColor
BackgroundColor
HostWidth
Posible colors: System.ConsoleColor( Black, DarkBlue, DarkGreen, DarkCyan, DarkRed, DarkMagenta, DarkYellow, Gray, DarkGray, Blue, Green, Cyan, Red, Magenta, Yellow, White
 

set-hostproperty -BackgroundColor "blue";

set-hostproperty -ForegroundColor "red";

get-item master:\content\home;

- Using .net Objects
[Sitecore.DateUtil]::FormatIsoDate($_.__Updated)

- Show calculated properties
get-childitem-recurse master:\\content | format-table Name, "__Updated By" , @{Label="Modified"; Expression={ [Sitecore.DateUtil]::FormatIsoDate($_.__Updated) } }

- Filter items
get-childitem-recurse master:\\content | where-object {[DateTime]::Now.Subtract([Sitecore.DateUtil]::IsoDateToDateTime($_."__Updated")).Days-lt 5} | format-table Name, "__Updated By" , @{Label="Modified"; Expression={ [Sitecore.DateUtil]::FormatIsoDate($_.__Updated) } }

- Create a new item
new-item-path"master:\content\home\testscript2" -Type "master:\templates\Sample\Sample Item";

- Modify a filtered list of items
foreach ($iinget-childitem-recurse master:\\content | where-object {[DateTime]::Now.Subtract([Sitecore.DateUtil]::IsoDateToDateTime($_."__Updated")).Days-lt 5})
{
    $i.BeginEdit();
    $i.Name =$i.Name +" "+"test";
    $i.EndEdit();
}

- Change the template of an item
I've try lots of combinations, but didn't find the way of make it work

One sample is this:
http://www.techphoria414.com/Blog/Change-Item-Templates-With-Sitecore-PowerShell.aspx

My problem is always the same, I'm not able to get the template item, it is always null.