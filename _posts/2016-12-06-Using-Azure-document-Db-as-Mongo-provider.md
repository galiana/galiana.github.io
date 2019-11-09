---
layout: post
title:  "Using Azure document Db as Mongo provider"
date:   2016-12-06 11:34:10 +0100
categories: [Sitecore, Azure, DocumentDB]
---

Since Document DB started supporting Mongo's protocol I was wondering if we could have Sitecore running on Document DB.

And today I managed to get it running.<!--more--> It threw a couple of exceptions the first time, but since then I don't see any error and, apparently, everything is working fine. (Of course, I Haven't tested it thoroughly).

## The first step is creating the Database as a service for mongo on Azure
Go to the portal and click on New

Filter by "mongo"

Select Database as a service for MongoDB

Fill in your details for name, subscription, resource group and location

![New CosmoDB](/static/img/_posts/newcosmodb.png  "New CosmoDB")


## Create the required databases
Select the Resource you've just created. On the overview tab, you can only find the option to add collections. We don't need to create each of the collections, Sitecore will do it for use, but what we have to do is create the database.

Go to the "Browse" section. Now you will see the option to "create database" on the top of the new blade. Create 4 new databases:

Analytics
Tracking_live
Tracking_History
Tracking_contact
 

![Add database to documentdb](/static/img/_posts/newdocumentdbdatabase.png  "Add database to documentdb")

## Update the connection strings
### Get the connection string from Azure.
Select the connection string option, from the settings section. Then copy the connection string from the bottom of the new blade

 
![DocumentDB connection string](/static/img/_posts/getconnectionstrings.png  "DocumentDB connection string")

### Replace your connection strings
 Open your connection string file: ...Website\App_Config\ConnectionStrings.config

Replace the connectionString for the connection analytics with the value you just copied from the portal. You will notice that this connection string is missing the database. To include the database, find the text "/?ssl=true" at the end of the string. We have to include the database after the "/". It should look like this: "xxxxxx/analytics?ssl=true";

Do the same for the other 3 databases.

Save the file.

Sitecore will re-start, but will throw errors like "Unable to connect to server Authentication failed because the remote party has closed the transport stream". This is because the mongo provider is not using configuring SSL correctly. We are lucky, Sitecore gives us the opportunity to set up the driver, before it uses it.

## Set up mongo provider to use SSL "Properly"
You can follow these two links to implement your own solution or you can use mine from here. 

Just drop this dll in the bin folder, and this config file in the app_settings/include folder

That's it. Now your Sitecore will connect to DocumentDB and will start creating the required collections. 

DocumentDB with support for mongo is still in preview, and this provider is not supported by Sitecore, so You shouldn't use it for production environments but if you are "playing" with Sitecore on Azure and you want to keep everything in Azure as Sitecore is doing with 8.2.1 supporting Azure webapps, Azure search, Azure Redis cache, etc.  with this trick, you can now use DocumentDB to store your collection database

Please, let me know if you find errors using DocumentDB