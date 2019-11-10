---
layout: post
title:  "Enabling remote desktop on Sitecore farms on Azure PaaS"
date:   2013-04-23 11:34:10 +0100
categories: [Sitecore, Azure]
---
The first thing you need is to set up the service configuration file to load the "Remote desktop service".
<!--more-->
Doing it with the sitecore azure module 2.0 is easy; navigate to the item /sitecore/system/Settings/Azure/Environments/Your environment/your farm

Edit the field service definition.

Under the imports sections of the webrole, add these lines:

<Import moduleName="RemoteAccess" />
<Import moduleName="RemoteForwarder" />

Now your service is able to execute the remote desktop, but now you need to configure how to run it.

The first thing you need is a certificate, if you don't have a proper certificate, you can create one with IIS, following [these steps](https://technet.microsoft.com/en-us/library/cc753127%28v=ws.10%29.aspx)

Next, export the certificate using the Export option of IIS. Create and write down your password.

Once you have the certificate, upload it to your cloud service following [this guide](https://www.windowsazure.com/en-us/manage/services/cloud-services/how-to-create-and-deploy-a-cloud-service/#uploadcertificate). Yes, you're right, you need to deploy first without remote desktop to create the Cloud service and then, upgrade files in order to enable remote desktop.

Once the certificate is uploaded, the certificate manager will show you a thumbprint. Copy it to some temp file, you'll need it.

Now it's time to update the field "Service configuration:

Under the configurationsettings of the role, add these three lines:

<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="yourusername" /> Don't forget to change the username!

The next line is used to specify the password of your remote desktop user, but this password must be encrypted first. You'll find how to do it [here](https://msdn.microsoft.com/en-gb/library/windowsazure/hh403998.aspx). Remember to use the thumbprint saved before.
So, now, you can add the next line under the settings:

<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value=youreallylongencryptedpassword" />

And the last line you've to add to your settings is, the expiry date for your user:

<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="2029-12-31T23:59:59.0000000-08:00" />

We're almost there... Time to tell your service to use the certificate previously uploaded:
Under the <instances> section, add the next line:

<Certificates>
<Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="yourthumbprint" thumbprintAlgorithm="sha1" />
</Certificates>

So this is it, now, "upgrade files" on your farm, and after the transitioning, you'll be able to connect to your VM. How?
Connect to the azure manager, select your cloud service, go to instances tab, select one, and check the bottom of the page, you'll see a "Connect" button. Click it and it will download an rdp file to connect to your VM.
Keep in mind you're not an administrator with full rights, and things are not always we're you expect...