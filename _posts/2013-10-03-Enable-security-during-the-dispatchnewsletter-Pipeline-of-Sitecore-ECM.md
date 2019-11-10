---
layout: post
title:  "Enable security during the dispatchnewsletter Pipeline of Sitecore ECM"
date:   2013-10-03 11:34:10 +0100
categories: [Sitecore, ExM]
---

It's seems that during the dispatchnewsletter pipeline the code is execute inside a securitydisabler block / context, so any effort to change the user context with a userswitcher is useless, as even changing the switcher, the security is not enabled, so you user is "administrator" at this point.<!--more-->

In my case, I was trying to check if any subscriber had no access to an specific item, to remove it from the list but as security is disabled, my code was not working.

So solution, execute your check inside a SecurityEnabler block / context:
{% highlight c# %}
using (new SecurityEnabler())
{
    foreach (Contact contact in contacts)
    {
        //Execute some check. By example:
        if(AuthorizationManager.IsDenied((ISecurable)youritem, AccessRight.ItemRead, (Account)contact.InnerUser))
        {
            //do some stuff
        }
    }
}
{% endhighlight %}

I Hope it helps!