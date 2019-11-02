---
layout: post
title:  "Sitecore and Solr: searching empty fields"
date:   2017-06-013 16:34:10 +0100
categories: [Sitecore,Solr]
tags: []
image: publish context.png
---
By default, when we index our content in solr (same for Lucene) Sitecore creates a document per item, and store the value of each (Sitecore) field in its related field in Solr. This is just the basic scenario, and can (And will) become more complex as we introduce other scenarios, but it's enough for our matter.

In Solr and Lucene, each document is different. A document can have multiple fields, the same field can be stored several times and a field can not exist for a given document.

By default, if our Sitecore field is empty, Sitecore won't create the field in Solr, making it difficult (if possible at all) to search for things like "where myfield is empty". The usual best practice for handling this scenario is to store an "empty" value in our Solr field, to be able to look for it.

Thanks to [this](https://sitecore.stackexchange.com/a/73/151) post I learned that now Sitecore has implemented this best practice, so we can specify the value to be inserted in the index for null values or empty strings, which is great but...

The implementation of the field reader for multivalue fields, prevents this mechanism to work. This field reader always returns a list, hence the document builder never uses any of the empty/null values assigned in the configuration.

With a few lines of code, o replaced the default reader with my own implementation where I check for empty lists before returning it to the document builder. If the list is empty, I return an empty string instead, "forcing" the document builder to use the configured emptyValue for each field.

This is my field reader:

{% highlight c# %}

public class MultiListFieldReader : global::Sitecore.ContentSearch.FieldReaders.FieldReader
    {
        public MultiListFieldReader()
        {
        }

        public override object GetFieldValue(IIndexableDataField indexableField)
        {            
            List<string> strs = base.GetFieldValue(indexableField) as List<string>;            
            if (strs == null || !strs.Any())
                return string.Empty;
            else
                return strs;
            
        }
    }
 
{% endhighlight %}

And this is how we can replace the default reader for multivalue fields

{% highlight xml %}

<fieldReaders type="Sitecore.ContentSearch.FieldReaders.FieldReaderMap, Sitecore.ContentSearch">
            <mapFieldByTypeName hint="raw:AddFieldReaderByFieldTypeName">
              <fieldReader fieldTypeName="checklist|multilist|treelist|treelistex|tree list" fieldNameFormat="{0}" fieldReaderType="ClearPeople.Search.Crawler.FieldReader.MultiListFieldReader, ClearPeople.Search.Crawler" patch:instead="fieldReader[@fieldTypeName='checklist|multilist|treelist|treelistex|tree list']"/>            
            </mapFieldByTypeName>
          </fieldReaders>
{% endhighlight %}

And finally, as per the previous blog posts, this is how we can configure the value for empty fields (Please not than even when we only need the emptyString attribute, we need to specify the nullValue, for Sitecore to call the right overload of the configuration method:

{% highlight xml %}
<fieldMap>
 
            <fieldNames hint="raw:AddFieldByFieldName">
              <fieldType fieldName="excludefromsearch"     returnType="string" />
              <field returnType="stringCollection" fieldName="sectors" nullValue="none" emptyString="none" />
            </fieldNames>
          </fieldMap>
{% endhighlight %}
I have raised a ticker with Sitecore to verify this behaviour and confirm a valid solution, in the meantime, use this with care and always do your test before going live with it.