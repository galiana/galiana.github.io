---
layout: post
title:  "How to support diacritic search on Sitecore 7 (Search with special characters)"
date:   2014-04-06 11:34:10 +0100
categories: [Sitecore, Lucene]
---

As you know, sitecore search is based on Lucene. Lucene uses Analysers and filters in order to "read" and "query" the content. By default sitecore 7.0 (131127) is configured to use by default, the Lucene standard analyser.<!--more--> This analyser splits the content into different tokens and remove some English stop words.

To be able to return the same items using or not diacritics, we need to force Lucene to treat this different combinations of characters as the same. To do this, Lucene already provides a filter called ASCIIFoldingFilter. So, what do we need to do?

- Create a  custom class, inheriting from the Standard Analyser
- Include the filter ASCIIFoldingFilter
- Change the default analyser by our custom analyser.
- Rebuild your index
 

Steps 1 and 2: Use [This](https://github.com/galiana/Sitecore7customsearch/blob/master/AsciiAnalyzer.cs) a sample of a custom analyser , based on the StandardAnalyzer using the ASCIIFoldingFilter

Step 3: To change default index patch the file \App_Config\Include\Sitecore.ContentSearch.Lucene.DefaultIndexConfiguration.config modifying the section sitecore\contentsearch\configuration\defaultindexconfiguration\analyzer
Change the type of the second sub item param (by default Lucene.Net.Analysis.Standard.StandardAnalyzer, Lucene.Net) by your new class .

That´s it. Now sitecore will "know" that a=á!


Step 4: The last step is rebuild your indexes to be sure that the content of the index is updated.

Keep in mind, we´ve modified the default index analyser, but this can be overridden in different places, like in the field definitions.