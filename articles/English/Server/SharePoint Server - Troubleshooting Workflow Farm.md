---
layout: page
title: 'Office Online Server Troubleshooting in Sharepoint Environment'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-01-20'
---


<sup>
Not all of the steps mentioned below should be executed in order to successfully solve SharePoint workflow issues. The steps below may not be applicable to your Farm, configuration, scenario or business needs. Some of them may affect uptime or server performance. The article serves merely as a list of possibilities and not as a pre-defined solution. Please choose carefully steps applicable to your situation and make sure you are aware of possible consequences before applying any of them.  
</sup>



<h1>Workflow and Service Bus Farm Status</h1>
A quick way to verify the status of your farm is to use Powershell cmdlets:

<h4>Get-WFFarm</h4>
This will give you an overview of your Workflow Farm configuration. Verify that the databases in Data Source, RunAsAccount, ports, certificates, and other settings are as expected:

<img src="/articles/images/Server/WFServer1.png" width="400">

<h4>Get-WFFarmStatus</h4>
Verify that the services are running: 

<img src="/articles/images/Server/WFServer2.png" width="400">

If the services are stopped, a cmdlet that might be helpful is ```Start-WFHost``` (no parameters).

<h4>Get-SBFarm</h4>
Like for the workflow farm, verify that service bus farm configuration corresponds to your expectations.

<h4>Get-SBFarmStatus</h4>
Verify that the services on the service bus farm are running:

<img src="/articles/images/Server/WFServer3.png" width="400">

<h4>Start-SBHost vs Start-SBFarm</h4>
You can use ```Start-SBHost``` to start Service Bus for Windows Server and dependent services that run on the node.  ```Start-SBFarm``` will start all Windows services for Service Bus for Windows Server on all farm nodes. Use ```Start-SBFarm``` after performing configuration changes and updating all the hosts. You can run this cmdlet **only** from a machine that is already a part of Service Bus for Windows Server farm.[[1]](https://learn.microsoft.com/en-us/previous-versions/azure/jj248756(v=azure.10)?redirectedfrom=MSDN)[[2]](https://learn.microsoft.com/en-us/previous-versions/azure/jj248761(v=azure.10)?redirectedfrom=MSDN)
Be aware that restarting the services may cause some delays: [https://blogs.msdn.microsoft.com/feseca/2014/11/19/service-bus-message-broker-service-takes-5-minutes-to-start/](https://blogs.msdn.microsoft.com/feseca/2014/11/19/service-bus-message-broker-service-takes-5-minutes-to-start/) 

<br/><br/>
<h1>Services</h1>
Verify that the services associated with Workflow Manager Farm are running: 

* Workflow Manager Backend
* Service Bus Message Broker
* Service Bus Gateway
* Windows Fabric Host Service (FabricHostSvc)
  
You can try restarting them. If Workflow Manager Backend is stopped or missing:
```
net start WorkflowServiceBackend
```

<br/><br/>
<h1>Service Applications</h1>
 To function correctly SharePoint 2013 Workflows require App Management Service and Site Subscription Service to be provisioned. It is not required to set up a wildcard certificate and DNS registration but both instances need to be running.[[3]](https://learn.microsoft.com/en-us/sharepoint/governance/install-and-configure-workflow-for-sharepoint-server) Verify the following Service Applications. Additionally, you can check logs for potential errors:

* App Management Service
* Site Subscription Service
* Workflow Service Application Proxy
  
When you click on Workflow Service Application Proxy you should see the Workflow Service Status with the message: *Workflow is Connected*

<img src="/articles/images/Server/WFServer4.png" width="400">


Additionally you can try to create a workflow with SharePoint Designer and see if the option to create 2013 workflows appears in the drop-down menu.


If the status shows a message in red about a missing connection, you may try to [Register-SPWorkflowService](https://docs.microsoft.com/en-us/powershell/module/sharepoint-server/register-spworkflowservice?view=sharepoint-ps):

```powershell
Register-SPWorkflowService -SPSite https://site_name -WorkflowHostUri https://workflow.contoso.com:12290 -ScopeName SharePoint
```
or
```powershell
Register-SPWorkflowService -SPSite https://site_name -WorkflowHostUri https://workflow.contoso.com:12291 -ScopeName SharePoint -AllowOAuthHttp
```

<br/><br/>
<h1>Databases</h1>
There are 3 WF databases and 3 SB databases created during creation of the Workflow Farm:

<img src="/articles/images/Server/WFServer5.png" width="400">



Verify (e.g. in SQL Management Studio) that all the databases exist, do not indicate any issues and have appropriate permissions.

For the exact process of WF Farm creation please refer to excellent materials from Spencer Harbar and Wictor Wilen: 

[http://www.harbar.net/articles/wfm2.aspx](http://www.harbar.net/articles/wfm2.aspx)

[https://channel9.msdn.com/Events/SharePoint-Conference/2014/SPC356](http://www.harbar.net/articles/wfm2.aspx)


<br/><br/>
<h1>Responses</h1>
Verify that your workflow responds under the assigned url:

<img src="/articles/images/Server/WFServer6.png" width="400">

<br/><br/>
<h1>Updated Certificates</h1>
When you have just updated your certificates, you may want to try the following steps:

* Restart IIS on each of the SharePoint WFEs
* Force the immediate run of the "Refresh Trusted Security Token Services Metadata" timerjob
* Add the Workflow Manager Certificate to SharePoint’s trust: [https://technet.microsoft.com/en-us/library/jj658589.aspx](https://technet.microsoft.com/en-us/library/jj658589.aspx)

Sources:
[https://blogs.msdn.microsoft.com/whereismysolution/2017/02/08/changing-my-workflow-manager-farm-certificates/](https://blogs.msdn.microsoft.com/whereismysolution/2017/02/08/changing-my-workflow-manager-farm-certificates/)
[http://www.harbar.net/articles/wfm3.aspx](http://www.harbar.net/articles/wfm3.aspx)


<br/><br/>
<h1>System Account</h1>
Verify that the SharePoint workflows are not running as a system account. Using system account to run your SharePoint workflows may lead to errors such as "You do not have permissions to do this operation".


<br/>
<h1>See Also</h1>
[https://social.technet.microsoft.com/wiki/contents/articles/29158.workflow-manager-disaster-recovery.aspx](https://social.technet.microsoft.com/wiki/contents/articles/29158.workflow-manager-disaster-recovery.aspx)
[http://www.harbar.net/](http://www.harbar.net/)


<h1>Other languages</h1>
<a href="https://social.technet.microsoft.com/wiki/contents/articles/51976.sharepoint-20132016-troubleshooting-une-ferme-de-workflow-fr-fr.aspx"> SharePoint 2013/2016 : Troubleshooter une ferme de workflow (fr-FR)</a>
