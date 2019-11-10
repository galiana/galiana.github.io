---
layout: post
title:  "Find out why your sitecore azure upgrade is failing with errors"
date:   2013-10-03 11:34:10 +0100
categories: [Sitecore, Azure]
---
Recently, I tried to upgrade my sitecore CD server with the Upgrade Files option of Sitecore Azure Module 2.0, but after creating the package it shown an time out error:<!--more-->

![Azure upgrade deployment error](/static/img/posts/sitecoreazuremoduleerror.png "Azure upgrade deployment error")

Initializing... failed System.ApplicationException: myinsntance [S] Initializing... failed ---> System.ApplicationException: Initializing... is failed ---> System.ApplicationException: Time Out, Upgrade configuration was not initiated. at Sitecore.Azure.Managers.Pipelines.UpgradeDeployment.WaitForInitialization.Action(RolePipelineArgsBase arguments) at Sitecore.Azure.Managers.Pipelines.BasePipeline.RolePipelineProcessor.Process(RolePipelineArgsBase args) --- End of inner exception stack trace --- --- End of inner exception stack trace ---

This error was not enough to find a solution, so I had to go to the Azure manager portal, select the "management Services" on left hand column. This section gives you information about your azure operations.

Then, on the top menu, select "Operation Logs", and select the last "Upgrade deployment". last step, look at the bottom of the page and clic "Details"; this will open a pop-op dialog, with the detailed xml with the error related to your operation under "Operation Status" => "Error".


![Azure management portal](/static/img/posts/azuremanagementportal.png "Azure management portal")

In my case, my error was "The size of the package is 645717899 bytes, which is too large".

My Solution? Go to the instance with the azure module, delete the content of the media cache folder (530MB) and done. Launch the upgrade files again, and in a "moment" the CD server will be upgraded.

Good Luck!