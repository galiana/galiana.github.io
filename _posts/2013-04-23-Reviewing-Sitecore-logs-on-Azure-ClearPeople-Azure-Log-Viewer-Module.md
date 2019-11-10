---
layout: post
title:  "Reviewing Sitecore logs on Azure. ClearPeople Azure Log Viewer Module"
date:   2013-04-23 11:34:10 +0100
categories: [Sitecore, Azure]
---

When you use the Module Sitecore Azure 2.0 to move your Sitecore web site to Azure, you lose the log files and the old log viewer become useless. <!--more-->Now, the information is stored in the table WADLogstable, in the table storage. The idea behind this change is to be able to persist your logs, even if the vm supporting you application is replaced.

In order to review your logs, you should use a storage explorer  as "Azure storage explorer", or use the Azure Table Storage Driver of Linq Pad 4, etc. Due to the storage limitations, these applications are really uncomfortable when you're trying to find a problem, as is quite difficult (But possible) to filter by a range of dates, and it's not possible to do a wildcard search.

In order to make our lives a bit easier we've create the [ClearPeople Azure Log Viewer](http://marketplace.sitecore.net/en/Modules/Sitecore_Azure_Log_viewer.aspx). This module integrates within the Sitecore Azure Module 2.0 UI, and gives us the Sitecore logs and the windows events from within Sitecore and allows us to execute server searches by a date range, log level and instance name and then filter the results dynamically as we type.

## Requisites
- This module has been built on top of Sitecore Azure Module 2.0, so it's the only requisite.
## Installation
- Download the package and install it as any other package with the Installation wizard.
## Configuration
You can modify several settings of the module editing the file App_Config\Include\ClearPeople.Azure.config
- ClearPeople.Azure.LogsViewer.MaxRowsPerRequest: This is the maximum number or records the application will request to the table storage. Usually, this is the maximum supported by the provider.
- ClearPeople.Azure.LogsViewer.LogsTableName: This is the name of the table used to storage the logs.
- ClearPeople.Azure.LogsViewer.WindowsEventsTableName: This is the name of the table used to storage the windows events.
- ClearPeople.Azure.LogsViewer.NumberofExtraPagesToCache: The application will download extra pages and store them in cache.
- ClearPeople.Azure.LogsViewer.DefaultHoursToGet: By default, the UI will get the logs from last x hours
- ClearPeople.Azure.LogsViewer.FullyCachedTableSummaryText: Text to show in the summary when we've downloaded all the records available
- ClearPeople.Azure.LogsViewer.CachingRowsPendingTableSummaryText: Text to show in the summary when there are pending rows to download.
- ClearPeople.Azure.LogsViewer.StoageConnectionString: Connection string template
- ClearPeople.Azure.LogsViewer.JQueryUIThemeUrl: URL of a jQueryUI theme
- ClearPeople.Azure.LogsViewer.DialogWidth and ClearPeople.Azure.LogsViewer.DialogHeight: Dimensions of the dialog
- ClearPeople.Azure.LogsViewer.BindGridOnLoad: Set this property to true to get logs when the application is opened. By default, the last two hours.
- ClearPeople.Azure.LogsViewer.NoRecordsFoundText: Text to show when the server can't find any record for the selected filter.
- ClearPeople.Azure.LogsViewer.LoadingText: Text to show while the application is busy.

## Using the module

Open the Sitecore Azure module, and click on one of your farms. Two new options will appear at the bottom of the context menu.
![ClearPeople Azure logs Module: farm contextual menu](/static/img/posts/ClearPeopleAzurelogsModulefarmcontextualmenu.png "ClearPeople Azure logs Module: farm contextual menu")

Here you can choose if you want to review Sitecore Logs, or Windows events. Both options will open the same interface, with the proper filter applied.
![ClearPeople Azure logs Module: date time picker for ranges](/static/img/posts/ClearPeopleAzurelogsModuledatetimepickerforranges.png "ClearPeople Azure logs Module: date time picker for ranges")

Use the date time pickers to choose the period you are looking for. Choose a maximum log level if you don't want to get everything, the instance, if you need to filter per VM, and your desired number of rows and click "search". After a few seconds your logs will be there.

![ClearPeople Azure logs Module: Local filter as you type](/static/img/posts/ClearPeopleAzurelogsModuleLocalfilterasyoutype.png "ClearPeople Azure logs Module: Local filter as you type")
 
If you're looking for a specific entry in the logs, click on "Filter current page", and type into the new text box. The filter will be applied instantly to the current page.

You'll find a pagination control at the bottom of the table. You'll notice that the number of pages could increase while you're navigating through them. This is a Table storage limitation as you don't know the number of records until you get the last.

Each different query will cache its own results, so if you go forward and backward through the pages, it won't create new request to the storage server.

## Notes:

This is an initial version created for internal use. If you find any bug (you will), please help us to improve this tool sending your comments.
Don't install it directly in a production server, test is first. It won't damage your logs, but it will modify your settings and bin directory, so something could go wrong.
Keep in mind that queryng the table storage and downloading records has a cost.
Install the module at your own risk; we're not responsible of any damage it could cause.
The source code will be available soon.

## Functionality to be added on future releases:
- Export logs to file
- Identify rows by colour based on the log level