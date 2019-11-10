---
layout: post
title:  "Enabling Sitecore log files on Azure PaaS"
date:   2013-04-24 11:34:10 +0100
categories: [Sitecore, Azure]
---
As we commented on previous post the default configuration of the Sitecore Azure module 2.0 moves the log files to table storage.<!--more-->

We agree with this solution, as it's the best practice for Azure, but sometimes, it's easier to catch your bug, if you've the usual log files, even if you installed our ClearPeople Azure log viewer, by example is sitecore wants the logs of the last two days (Our ClearPeople Azure log Viewer will support this functionality shortly), so... what about sending the logs to table storage, but saving them to log files at same time?

It's easy, open your deplument item in the content tree and edit the field "WebConfigPatch" find the text. Find the log4net section and add this text under the Azure appender:
{% highlight xml %}
<appender name="LogFileAppender" type="log4net.Appender.SitecoreLogFileAppender, Sitecore.Logging">
    <file value="$(dataFolder)/logs/log.{{date}}.txt"/>
    <appendToFile value="true"/>
    <layout type="log4net.Layout.PatternLayout">
       <conversionPattern value="%4t %d{{ABSOLUTE}} %-5p %m%n"/>
    </layout>
</appender>
{% endhighlight %}

This is the standard appender, nothing new.

Now, find the text "<appender-ref ref="AzureAppender" />" and add the next line under it:
{% highlight xml %}
<appender-ref ref="LogFileAppender" />
{% endhighlight %}
That's it, you just configured the appender and told the engine to us both, so it will write to table storare and log file.

In order to apply these changes properly, you need to "upgrade files" on your farm.
