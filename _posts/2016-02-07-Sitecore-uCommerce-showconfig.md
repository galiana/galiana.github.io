---
layout: post
title:  "Sitecore uCommerce showconfig"
date:   2016-02-07 11:34:10 +0100
categories: [Sitecore, uCommerce]
---


Every Sitecore developer knows (or should know) about the configuration transformation files under the folder app_config/include.

When Sitecore starts, it merges those file into a single xml using custom transformation attributes to then, read its settings from there. We as developers, create new configuration files hoping that Sitecore understands them as we want. To make our life easier Sitecore provides a page to check the results of those transformations, so we can see the final result.

uCommerce follows a similar pattern: uCommerce configuration files are stored under \sitecore modules\Shell\uCommerce\Configuration. Here you'll find several files where uCommerce splits its configuration and where, again, we can introduce changes.

One of the differences is that uCommerce recommends adding any customization to the file custom.config, instead of using our own files.

Another difference is that uCommerce doesn't provide a page to display the combination of all the different xml files in one single view.

To fill this gap, and with a bit of reverse engineering I have created [this](https://github.com/ClearPeopleLtd/CommerceShowConfig/blob/uCommerece6/uCommerce%20ShowConfig/Sitecore%20packages/1.0/ClearPeople%20uCommerce%20Showconfig-1.0.zip) module available in the [marketplace](https://marketplace.sitecore.net/en/Modules/U/uCommerce_Showconfig.aspx), which does exactly that, display the same xml as uCommerce reads to load its configuration, so you can search in one place within uCommerce configuration.

Simply install the package on top of a Sitecore instance (tested with Sitecore 8) and uCommerce (tested with uCommerce 6.7) and then visit /sitecore/admin/showucommerceconfig.aspx

This is just a PoC I built after the online uCommerce training, maybe using the worst solution possible, but it seems to work and I had fun building it.

I'll try to improve it if anybody finds it useful. Any comments are welcomed!!!