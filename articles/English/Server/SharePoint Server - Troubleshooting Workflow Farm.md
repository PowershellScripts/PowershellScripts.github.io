<sup>
Not all of the steps mentioned below should be executed in order to successfully solve workflow issues. The steps below may not be applicable to your Farm, configuration, scenario or business needs. Some of them may affect uptime or server performance. The article serves merely as a list of possibilities and not as a pre-defined solution. Please choose carefully steps applicable to your situation and make sure you are aware of possible consequences before applying any of them.  
</sup>



<h1>Workflow and Service Bus Farm Status</h1>
A quick way to verify the status of your farm is to use Powershell cmdlets:

Get-WFFarm
This will give you an overview of your Workflow Farm configuration. Verify that the databases in Data Source, RunAsAccount, ports, certificates, and other settings are as expected:


Get-WFFarmStatus
Verify that the services are running. 


If the services are stopped, a cmdlet that might be helpful is Start-WFHost (no parameters).

Get-SBFarm
Like for the workflow farm, verify that service bus farm configuration corresponds to your expectations.

Get-SBFarmStatus
Verify that the services are running:


Start-SBHost vs Start-SBFarm
You can use Start-SBHost to start Service Bus for Windows Server and dependent services that run on the node.  Start-SBFarm will start all Windows services for Service Bus for Windows Server on all farm nodes. Use Start-SBFarm after performing configuration changes and updating all the hosts. You can run this cmdlet only from a machine that is already a part of Service Bus for Windows Server farm.[1][2]
Be aware that restarting the services may cause some delays: https://blogs.msdn.microsoft.com/feseca/2014/11/19/service-bus-message-broker-service-takes-5-minutes-to-start/ 


<h1>Services</h1>
Verify that the services associated with Workflow Manager Farm are running 

Workflow Manager Backend
Service Bus Message Broker
Service Bus Gateway
Windows Fabric Host Service (FabricHostSvc)
You can try restarting them. If Workflow Manager Backend is stopped or missing:

net start WorkflowServiceBackend

<h1>Service Applications</h1>
 To function correctly SharePoint 2013 Workflows require App Management Service and Site Subscription Service to be provisioned. It is not required to set up a wildcard certificate and DNS registration but both instances need to be running.[3] Verify the following Service Applications. Additionally, you can check logs for potential errors:

App Management Service
Site Subscription Service
Workflow Service Application Proxy
When you click on Workflow Service Application Proxy you should see the Workflow Service Status with the message: Workflow is Connected:




Additionally you can try to create a workflow with SharePoint Designer and see if the option to create 2013 workflows appears in the drop-down menu:
<image>

If the status shows a message in red about a missing connection, you may try to Register-SPWorkflowService:

Register-SPWorkflowService -SPSite https://site_name -WorkflowHostUri https://workflow.contoso.com:12290 -ScopeName SharePoint

or

Register-SPWorkflowService -SPSite https://site_name -WorkflowHostUri https://workflow.contoso.com:12291 -ScopeName SharePoint -AllowOAuthHttp


<h1>Databases</h1>
There are 3 WF databases and 3 SB databases created during creation of the Workflow Farm:





Verify (e.g. in SQL Management Studio) that all the databases exist, do not indicate any issues and have appropriate permissions.

For the exact process of WF Farm creation please refer to excellent materials from Spencer Harbar and Wictor Wilen: 

http://www.harbar.net/articles/wfm2.aspx

https://channel9.msdn.com/Events/SharePoint-Conference/2014/SPC356


<h1>Responses</h1>
Verify that your workflow responds under the assigned url:



<h1>Updated Certificates</h1>
When you have just updated your certificates, you may want to try the following steps:

Restart IIS on each of the SharePoint WFEs
Force the immediate run of the "Refresh Trusted Security Token Services Metadata" timerjob
Add the Workflow Manager Certificate to SharePoint’s trust: https://technet.microsoft.com/en-us/library/jj658589.aspx
Sources:
https://blogs.msdn.microsoft.com/whereismysolution/2017/02/08/changing-my-workflow-manager-farm-certificates/
http://www.harbar.net/articles/wfm3.aspx


<h1>System Account</h1>
Verify that the workflows are not running as a system account. Using system account to run your workflows may lead to errors such as "You do not have permissions to do this operation"


<h1>See Also</h1>
https://social.technet.microsoft.com/wiki/contents/articles/29158.workflow-manager-disaster-recovery.aspx
http://www.harbar.net/


<h1>Other languages</h1>
<a href="https://social.technet.microsoft.com/wiki/contents/articles/51976.sharepoint-20132016-troubleshooting-une-ferme-de-workflow-fr-fr.aspx"> SharePoint 2013/2016 : Troubleshooter une ferme de workflow (fr-FR)</a>
