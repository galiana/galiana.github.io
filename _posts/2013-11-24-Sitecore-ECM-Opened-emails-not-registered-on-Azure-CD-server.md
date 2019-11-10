---
layout: post
title:  "Sitecore ECM Opened emails not registered on Azure CD server"
date:   2013-11-24 11:34:10 +0100
categories: [Sitecore, ExM, Azure]
---
Sitecore Azure module 2.0 has an error on the default transformation code of Content delivery farms.<!--more--> In fact, Sitecore Azure module 3.0 inherited the same error in the Global web config patch.

When you combine this error with ECM, the result is that you don't get opened emails information when you set up your base url to your CD servers. Why? Becaouse the functionality to register this "event" uses your "Content database", and it is not available.

If we have a look at the transformation fields (or patch fields), we´ll find this section:
{% highlight xml %}
<xsl:template match="sitecore/sites/site[@content='master']/@content">
    <xsl:attribute name="value">web</xsl:attribute>
  </xsl:template>
{% endhighlight %}
Looks fine right? This changes the content database of any site from master to web, exactly what we need. Are you sure?

The result of this transformation is site name="xxx" value="web"

So, how can we fix the problem?
{% highlight xml %}
<xsl:template match="sitecore/sites/site[@content='master']/@content">
    <xsl:attribute name="content">web</xsl:attribute>
  </xsl:template>
 {% endhighlight %}
As easy as that. replace "value" by "content", save, upgrade files and problem fixed, now you can see who opened your emails.

This error is not only related to ECM, as this will break any site using the Content Database