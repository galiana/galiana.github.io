---
layout: post
title:  "Learnt at Sitecore Symposium Barcelona 2014 #symeu From placeholders to the experience database"
date:   2014-10-09 11:34:10 +0100
categories: [Sitecore]
---

## First conclusion: Sitecore is not a (web) content management system.

"What did you drink?" you'll be thinking… but this is one of the key concepts the Sitecore guys managed to convince me of: Sitecore is a marketing platform, which includes a (very good) CMS. If you think about it, it makes sense…

Yes, sitecore has been one of the best CMS in the market, since they began, even patenting the concept of "placeholders" in the U.S.A. (I would have never image that, but they show us the documents).
When was last time you saw a demo or presentation by sitecore about CMS capabilities? It's always about personalization, about personas, about goals, point, etc.
Why DMS? Why "Digital marketing suite" and not just "analytics"?
Why ECM (Email campaign manager), integrations with CRMs, integrations with different ecommerce platforms?
Why most of these terms are related to marketing and not technology?
The answer is easy, because Sitecore abandoned the focus on the CMS years ago, and they changed their focus in transforming the CMS into a complete marketing platform, not only able to deliver content for the web, but to deliver the proper content, at the proper time, in the proper channel, for each user.

## How has sitecore achieved it?
Of course, it has not been easy, but the concept is pretty simple, track every single interaction of the users with the system, and weight it against different goal.

The architecture of the system, allowing different topologies and a huge scalability to support heavy loads of usage. CM,CD, different databases, in different servers, etc.

The data structure implemented by sitecore was great to support any data structure of the content

Storing user interactions with OMS and then DMS, a relation database able to store structured data, per user, content, goal, etc. This was the core of the "personalization engine" as for the different reporting tools.

Adding new channels to the suit: Email, ecommerce, social networks, CRM, etc

This is Sitecore 6, this is the past and the sitecore most of us know and are comfortable with, but it was not enough to keep up to date with the new demands of the era: so sitecore had to step up to the net level, this is Sitecore 8, this is the future:

Even when sitecore already support IaaS with Azure and Amazon, and has specific tools to support PaaS on Azure, sitecore will soon support SaaS, supporting all the different architectures commonly used today. First problem solved: Take it to the cloud.

The extremely customizable data model was limiting the capability to manage huge amount of data, so with Sitecore 7 the concept was changed. The data is still stored in databases and tables, but now the focus is to use if from indexes by taking the old and simple implementation of Lucene, to a provider model, with a much more powerful API, supporting not only a newest version of Lucene, but a much more scalable SOLR. Second problem solved: Manage any amount of content.

Sitecore 7.2, came as a treat for developers, with some if the feature the community has been demanding, like parallel publish.

## What problem will sitecore solve next:
How to store huge amount of user's interactions and process it quick enough to provide that perfect piece of content, in that right moment. The solution? The experience database coming in Sitecore 7.5: A combination of a mongoDB database, moving from the relational database of DMS, to a NOSQL database where to store massive amount of data, quickly and really fast read access to the original document (yes to the original document no more mappers). This data is queued and processed, with the well known pipelines that we can (must) extend to transform this to a data warehouse for reporting services. We will be able to manage our own experience database, but Sitecore will provide the option to host it for us (Apparently on Azure)

## The next challenge:
make it look good. The desktop, content tree, etc was an amazing UI years ago, but completely out dated nowadays. With sitecore 7, SPEAK came to life, and sitecore 8, has been rebuild on top of it, providing a new fresh, modern UI, with a real API, following the latest patterns and practices, been a real platform to be extended. So there´s no doubt, we need to learn SPEAK and we have to do it now.

## More?

- Personalised content is based on predefined rules (for this type of user, use this data), but that means marketers must guest who theirs users are, and explicitly decide what content to show. Now with the Skynet project, sitecore is able to process all that amount of data of the experience database and use the latest [Machine learning](https://azure.microsoft.com/en-us/services/machine-learning/) system and data models to automatically suggest and apply new rules. This means, finally sitecore will decide, on the fly the perfect content for the each user at this specific moment.

- A/B testing: every time for everything. Testing your content is not trivial anymore. Sitecore is moving towards a culture of testing everything, and learning from the test and from the different editors to predict the results and improve the personalization. Editor will be ranked based on their predictions, not only to make it funnier, but to include these results in the learning models. Do not test for a certain amount of time of visit, now you can test until the system is sure that the results are valid, letting you decide "how much sure".

- Understanding the actions of your users is vital to guide them toward your goals. The new path analyser implements an event sequence analyser model integrated with Machine learning services to represent graphically the different actions of clients, representing the commons path and their values through the content, helping you to understand you user´s and implementing the necessary changes to guide them towards your goals.

- Dynamically segmented recipient list for ECM. Forget about sending emails to everybody in a recipient list or having to filter them manually, or with custom code. The next version will use the new power of Sitecore to let you segment your recipient list on the fly.

- eCommerce integration usually was based on a ecommerce platform having to integrate their API with the Sitecore API, so when you have to implement it and extend it, means you have to use both of them, the better you can. Sitecore has change this, introducing the Sitecore commerce connector. Now sitecore provide a commerce provider definition, which the different ecommerce solutions have to follow to integrate with sitecore, meaning that no matter which solution you use, it will be managed with the same API.

## Are you not convinced yet Sitecore is not just a CMS?

What if I tell you can use all the power described above to track and inject content or functionality into any other website? No matter if it´s a static html, java, php, .net… with the new Federated experience manager and the experience editor, you´ll be able to edit any site, and use it to track users and fill the experience database and learn from them, inject content, inject personalized content, etc. and just with a line of JavaScript.

One thing is clear, we´ve a big challenge ahead of us: NoSQL databases, indexes, reporting services, cloud computing, machine learning services, event sequence analysers, data models, MVVM, JavaScript. Are you ready to take it?