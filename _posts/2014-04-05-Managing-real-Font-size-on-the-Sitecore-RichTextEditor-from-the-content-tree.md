---
layout: post
title:  "Managing real Font size on the Rich text editor of Sitecore from the content tree."
date:   2014-04-05 11:34:10 +0100
categories: [Sitecore]
---
Has shown on my previous post "[Using real pixels in the rich text editor of sitecore](/post/2013/09/19/Using-real-sizes-px-in-the-rich-text-editor-RTE-of-sitecore)" is it possible to change the relative font size in the RTE drop down<!--more-->, by real pixels, but this solution may not be enough, as this list is only manageable modifying a xml file, so if you want to add a new value, you need to update that file on the server.

The file is:sitecore\shell\Controls\Rich Text Editor\ToolsFile.xml

As described [here](https://stackoverflow.com/questions/22608644/add-font-size-in-sitecore-richtext-editor) you need to add to that file:
{% highlight xml %}
<root>
    <tools name="MainToolbar" enabled="true">
        <tool name="RealFontSize" />
    </tools>
    <realFontSizes> 
        <item value="8px"></item>
        <item value="9px"></item>
        <item value="10px"></item>
        <item value="11px"></item>
        <item value="12px"></item>
        <item value="13px"></item>
        <item value="14px"></item>
        <item value="16px"></item>
        <item value="18px"></item>
        <item value="20px"></item>
        <item value="22px"></item>
        <item value="24px"></item>
        <item value="26px"></item>
        <item value="28px"></item>       
        <item value="32px"></item>
        <item value="48px"></item>
        <item value="60px"></item>
        <item value="72px"></item>
        </realFontSizes>
</root>
 {% endhighlight %}

In order to move the settings back to the profiles folders on the core database, we need to use a custom "EditorConfiguration" class to fill the Real Font Sizes list of the Telerik control as the default one provided by Sitecore only fills the seconds list of relative font sizes.

I´ve created a simple sample, which reads the real font sizes from it´s own folder of the profile.

You can download a working package from [here](https://stackoverflow.com/questions/22608644/add-font-size-in-sitecore-richtext-editor), or get the full code from [GitHub](https://github.com/galiana/SitecoreEditorConfigurator)