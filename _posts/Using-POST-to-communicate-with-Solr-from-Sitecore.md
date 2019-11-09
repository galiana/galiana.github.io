---
layout: post
title:  "Using POST to communicate with Solr from Sitecore"
date:   2017-04-28 11:34:10 +0100
categories: [Sitecore, Solr]
---

Recently I have been having issues while trying to send really long queries to Solr from Sitecore. Sometimes my problem was Solr rejecting the request, other .Net itself complaining about the length of the uri.

Error: Invalid URI: The Uri string is too long.

The remote server returned an error: (414) Request-URI Too Long

After considering setting higher values on Solr configuration, the long-term solution seems to be to use the post method to send the request. This is now part of Solrnet, the .Net client used by Sitecore.

I got in contact with Sitecore, but while waiting I came up with this "solution":
## Create a new SolrStartUp class inheriting from the default one:
 {% highlight c# %}
public class PostSolrStartUp: DefaultSolrStartUp
    {
        protected override ISolrConnection CreateConnection(string serverUrl)
        {
            SolrConnection basecon = new SolrConnection(serverUrl) { Timeout = SolrContentSearchManager.ConnectionTimeout };
            
 
            FieldInfo field = typeof(DefaultSolrStartUp).GetField("solrCache", BindingFlags.Instance | BindingFlags.NonPublic);
            var basecache = field.GetValue(this);
            if (basecache != null)
            {
                basecon.Cache = (ISolrCache)basecache;
            }
            PostSolrConnection solrConnection = new PostSolrConnection(basecon, serverUrl);
            return solrConnection;
        }
    }
 {% endhighlight %}

Create a new Sitecore Initializer:
{% highlight c# %}
public class InitializeSolrProvider
    {
        public InitializeSolrProvider()
        {
        }

        public void Process(PipelineArgs args)
        {
            if (!SolrContentSearchManager.IsEnabled)
            {
                return;
            }
            if (!IntegrationHelper.IsSolrConfigured())
            {
                (new PostSolrStartUp()).Initialize();
                return;
            }
            IntegrationHelper.ReportDoubleSolrConfigurationAttempt(this.GetType());
        }
    }
{% endhighlight %}

Replace the default initializer with yours:
{% highlight xml %}
  <sitecore>
    <pipelines>
      <initialize>
        <processor type="ClearPeople.Sitecore.ContentSearch.SolrProvider.InitializeSolrProvider, ClearPeople.Sitecore" patch:instead="processor[@type='Sitecore.ContentSearch.SolrProvider.Pipelines.Loader.InitializeSolrProvider, Sitecore.ContentSearch.SolrProvider']" />
      </initialize>
    </pipelines>
  </sitecore>
{% endhighlight %}
That's it, wit these changes my Sitecore instance communicates with Solr using post.

Tricks:

You must add a reference to SitecoreContentSearch.SolrProvider
You need to reference Solrnet 4.0.2.002 I couldn't find a valid version on Nuget, as the 4.0.2 (beta) doesn't have the new PostSolrConnection class we need, So I had to fall back to the one provided with Sitecore.
Side effects found so far:

With get, when Solr returnes some errors due to invalid field names, the error could be found in the search log, now it appears in the general log
Disclaimer: Of course, this is some PoC, and you must test and improve it yourself before using it in a real project.

Update 09/06/2017: Sitecore has reported this as a bug and has a patch available that let you configure which method to use: GET or POST. The reference number for this patch is 166359 The patche uses the same solution as this code to connect to Solr via POST with the PostSolrStartUp class. This is how they introduced the configuration bit in the InitializeSolrProvider:
 
{% highlight c# %}
public void Process(PipelineArgs args)
{
    if (SolrContentSearchManager.get_IsEnabled())
    {
        string setting = Settings.GetSetting("ContentSearch.RequestMethod");
        if (IntegrationHelper.IsSolrConfigured())
        {
            IntegrationHelper.ReportDoubleSolrConfigurationAttempt(this.GetType());
            return;
        }
        if (!StringExtensions.IsNullOrEmpty(setting) && setting.ToLower() == "post")
        {
            (new PostSolrStartUp()).Initialize();
            return;
        }
        (new DefaultSolrStartUp()).Initialize();
    }
}
{% endhighlight %}