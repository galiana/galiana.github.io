---
layout: post
title:  "Setting up uCommerce on a Content delivery server"
date:   2016-02-13 11:34:10 +0100
categories: [Sitecore, uCommerce]
---

Installing uCommerce on a content delivery server is pretty simple, nothing new, just install the package on Sitecore and done.

Perfect! everything is up and running, but... what if now you follow the standard procedure of "security hardening" for a CD server?

One of the usual steps is to remove any link to the master database. You do it, and nothing happens, it may look lie if it works but... sometimes uCommerce must read items directly from the Sitecore tables and, by default it does it from the master database. One example is the confirmation emails, uCommerce will fail with the error: Could not find configuration node: databases/database[@id='master']

You can control from which database uCommerce reads the items by changing the property "nameofcontentdatabase" of the component "SitecoreContext". By default, this configuration is stored in the configuration file shell.config, but as usual, uCommerce recommends to apply those changes in the custom.config file, instead of modifying any uCommerce file directly.

Summary.

Copy the following code into custom.config
{% highlight xml %}
<!-- Services -->
		<component id="SitecoreContext"
			service="UCommerce.Sitecore.ISitecoreContext, UCommerce.Sitecore"
			type="UCommerce.Sitecore.SitecoreContext, UCommerce.Sitecore">
			<parameters>
				<backEndDomainName>sitecore</backEndDomainName>
				<nameOfContentDatabase>web</nameOfContentDatabase>
			</parameters>
		</component>
{% endhighlight %}
Restart your application
In my own opinion, we should always apply this change as our users are expecting to be able to edit in the master database without those changes being applied on the front end, and in scenarios like this one, the email would be generated with the content from master, which is not the expected behaviour.

P.S. Credits to uComerce support team for this solution