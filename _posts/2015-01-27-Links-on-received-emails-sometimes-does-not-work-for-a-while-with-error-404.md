---
layout: post
title:  "Links on received emails sometimes doesn´t work for a while with error 404"
date:   2015-01-27 11:34:10 +0100
categories: [Sitecore, ExM]
---
If you ever notice that links are not working when somebody receives an email sent by Sitecore Email campaign Manager, but after a while, without doing anything everything starts working, you may have a problem with the publishing queue.<!--more-->

During the dispatchnewsletter pipeline, ECM executes the processor deployanalytics before the email is sent, and if you followed the recommendations, it will have an auto publish action when it´s "approved".

In theory, the system publishes the analytic data before sending the email but, why if you have a "long" queue of items pending to be publish? The auto publish actions simple triggers the publish, but don´t wait for it to finish, so ECM will send the email without the analytics data been published to the analytics database. At this point, when somebody receives the email and click on a link, the redirect page will try to log the link, but as it can´t find the analytic data, will return a 404 error.

Solution: Simple, override the process method of the class Sitecore.Workflows.Simple.PublishAction and force the processor to wait until the publishing task finishes. Here you have an example of how to do it.
 {% highlight c# %}
public void Process(WorkflowPipelineArgs args)
{
    Item dataItem = args.DataItem;
    Item innerItem = args.ProcessorItem.InnerItem;
    Database[] targets = this.GetTargets(dataItem);
    Language[] language = new Language[] { dataItem.Language };
    Handle publishTask = PublishManager.PublishItem(dataItem, targets, language, this.GetDeep)innerItem), false);
    PublishStatus status;
    while ((status = PublishManager.GetStatus(publishTask)) != null && !status.IsDone)
    {
        Thread.Sleep(500);
    }
}
{% endhighlight %}

Disclaimer:

I found that in rare scenarios (once after 1 year) the publishing could get stuck, and after 2 hours sitecore release the task without publishing. In this rare case, we´ll have the same problem.