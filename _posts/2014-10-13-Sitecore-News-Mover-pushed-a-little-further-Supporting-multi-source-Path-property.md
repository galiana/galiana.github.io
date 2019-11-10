---
layout: post
title:  "Sitecore News Mover pushed a little further Supporting multi-source Path property"
date:   2015-01-27 11:34:10 +0100
categories: [Sitecore]
---

The good old "[news mover](https://marketplace.sitecore.net/Modules/News_mover.aspx)" (Or item mover) has been there helping in most of my project, so I thought it´s time to give something back.
<!--more-->
## Supporting multi-source Path property 
Until the pull request is accepted (If it´s accepted), you can find [here](https://github.com/ClearPeopleLtd/NewsMover) a slightly different version, which supports a new property "roots" per template.

This property is optional and accepts one or more item ID´s separated by "|". If this property is used, only descendants of any of these items will be moved.

This parameter is useful if you only want items under a specific folder to be moved into folders.

{% highlight xml %}
<!--

    Define a template configuration.
    @id: [required] Any item based on the configured template will be ogranized
    @sort: [optional] How to configure the sorting of 'folders' and the item (Ascending, Descending, null)
    DateField: [required] The field on the template where the date is set
    YearTemplate: [required] The template to use for creating year 'folders'
    MonthTemplate: [optional] The template to use for creating month 'folders'
    DayTemplate: [optional] The template to use for creating day 'folders'
    @formatString: [optional] The Year/Month/Day nodes support this attribute.                   It will control how to format the date for the name of item.  defaults - yyyy/MM/dd for year, month, day nodes respectivley
    @roots: [optional] One or many item id´s separated by | Only descendants  of any of these will be processed

 -->  

    <template id="user defined/newsarticle" sort="Descending">
        <DateField>releasedate</DateField>
        <YearTemplate formatString="yyyy">Common/Folder</YearTemplate>
        <MonthTemplate formatString="MMMM">Common/Folder</MonthTemplate>
        <DayTemplate formatString="dd">Common/Folder</DayTemplate>
        <Roots>{83BF7C68-04AF-4316-9B23-5CFA365EFB4A}|{83BF7C68-04AF-4316-9B23-5CFA365EDSD4A}</Roots>
    </template>
{% endhighlight %}