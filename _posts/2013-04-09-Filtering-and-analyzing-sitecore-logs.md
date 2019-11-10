---
layout: post
title:  "Filtering and analyzing sitecore logs"
date:   2013-04-09 11:34:10 +0100
categories: [Sitecore]
---
I´ve been using Log parer lizard thanks to The Client View for a while to review sitecore logs, but today I found it didn´t work for my instance with sitecore 6.6 and the Azure module logs, it was throwing any error, but any result either.<!--more--> After some review I found my logs files are slightly different to my usual files, these contain the date before the time on each line.

I don´t know the cause of this change, but as you can manage a full folder of log files at same time with this tool, having the date on each line make easy to identify exactly the moment when something happened. The problem is that as line format is different, the regex expression was failing. To fix the problem, just create a copy of the file C:\Program Files (x86)\LizardLabs\Log Parser Lizard\log4netconfig.xml, edit it and replace the current content with: *Remove the unnecessary line breaks on the xml

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:noNamespaceSchemaLocation=
"C:\src\LogParserCSWebServiceInputFormat\LogParserRegexInputFormat.xsd">
<regex>^(?<ProcessID>\d+|\w+\s#\d+)\s+(?<DateTime>(?:\d{4}-\d{2}-\d{2}\s\d{2}:\d{2}:\d{2}))\s+
(?<LogType>\w+)\s+(?<Message>.*)$</regex>
	<fields>
			<field name="ProcessID" type="String"/>
			<field name="DateTime" type="Timestamp" format="yyyy-MM-dd HH:mm:ss" />			
            <field name="LogType" type="String"/>
            <field name="Message" type="String"/>
	</fields>
</config>
<regex>
{% endhighlight %}

The next step is creating a new query, on the Imput log format, choose Multiline RegEx log4net imput format, and click on Query properties, change the config file on the dialog, with your new file, and that's it, you can type your query as always, simply change any reference to the field time, by datetime.

{% highlight sql %}
SELECT dateTime, message
FROM 'C:\Temp\logs\*.*'
where datetime
between timestamp('2013/03/25 12:00:00', 'yyyy/MM/dd hh:mm:ss') and 
timestamp('2013/03/25 17:00:00', 'yyyy/MM/dd hh:mm:ss')
order by DateTime asc.
{% endhighlight %}

Ah! i almost forgot, to change the format of the logs, for example to alter the format of the time or date time, just search the log4net section of your web.config file, and play with the conversionPattern of the LogfileAppender. You can find a list of options [here](https://logging.apache.org/log4net/release/sdk/log4net.Layout.PatternLayout.html)