<sup>Disclaimer: Not all of the steps mentioned below should be executed. The steps below may not be applicable to your Farm, configuration, scenario or business needs. Some of them may affect uptime or server performance. The article serves merely as a list of possibilities and not as a pre-defined solution. Please choose carefully steps applicable to your situation and make sure you are aware of possible consequences before applying any of them.  </sup>

<h1>Office Online Server</h1>
Office Online Server is an Office server product that provides browser-based file viewing and editing services for Office files. Office Online Server works with products and services that support WOPI, the Web app Open Platform Interface protocol, such as SharePoint Server and Exchange Server. An Office Online Server farm can provide Office services to multiple on-premises hosts, and you can scale out the farm from one server to multiple servers as your organization's needs grow. Office Online Server can be installed on virtual machines but requires dedicated servers that run no other server applications.


<h1>Possible errors</h1>

1. Errors on connecting to OWA
 

2. Errors on opening Word\Excel documents
Error Message : Sorry, there was a problem and we can't open this document. If this happens again, try opening the document in Microsoft Word.

3. Errors in the event viewer, such as
WebOneNoteWatchdog reported status for OneNoteMerge in category 'Ping'. Reported status: OneNote Merge Ping test failed. OneNoteMerge.exe not available to execute merge
Exception type: ExcelWebRendererException
Exception message: We couldn't find the file you wanted


Do not hesitate to expand this list :)

<h1>Verify the current status</h1>


<h3>Office Web Apps Powershell</h3>

Powershell can be useful to verify the current state of the machine. There officewebapps module contains useful cmdlets for troubleshooting and below few examples will illustrate how to use them to our advantage:
Get-OfficeWebAppsExcelBIServer Jump
Get-OfficeWebAppsExcelUserDefinedFunction Jump
Get-OfficeWebAppsFarm Jump
Get-OfficeWebAppsHost Jump
Get-OfficeWebAppsMachine Jump
New-OfficeWebAppsExcelBIServer Jump
New-OfficeWebAppsExcelUserDefinedFunction Jump
New-OfficeWebAppsFarm Jump
New-OfficeWebAppsHost Jump
New-OfficeWebAppsMachine Jump
Remove-OfficeWebAppsExcelBIServer Jump
Remove-OfficeWebAppsExcelUserDefinedFunction Jump
Remove-OfficeWebAppsHost Jump
Remove-OfficeWebAppsMachine Jump
Repair-OfficeWebAppsFarm Jump
Set-OfficeWebAppsExcelUserDefinedFunction Jump
Set-OfficeWebAppsFarm Jump
Set-OfficeWebAppsMachine Jump

<h3>Verify farm status</h3>

Use Get-OfficeWebAppsFarm to verify the overall farm status:
 

InternalURL is the fully qualified domain name (FQDN) of the server that runs Office Online Server, such as http://servername.contoso.com. ExternalURL is the FQDN that can be accessed on the Internet. Verify that the urls are accessible, especially if you are getting errors when connecting to OWA.

<h3>Get-OfficeWebAppsMachine</h3>

This cmdlet will display the status of the machine, including roles and its health. 
 

If you have more servers in your farm, you can see them by using (Get-OfficeWebAppsFarm).Machines cmdlet.


<h3>Get-SPWopiBinding</h3>

The cmdlet from SharePoint Management module returns a list of bindings that were created by using New-SPWOPIBinding on the current SharePoint farm where this cmdlet is run. Results include actions, applications, file types and zones that are configured for a WOPI application (such as a server that runs Office Web Apps Server). Verify these especially if only one parcticular file type is causing issues. 
 

<h3>Get-SPWOPIZone</h3>

This cmdlet returns the zone that is configured for the WOPI application (such as a server that runs Office Web Apps Server) to use. Return values may be "internal-http," "internal-https," "external-http" or "external-https." The zones define how SharePoint calls the Office Online Server url, e.g. internal-http will use http://owa instead of https://owa.domain.com. This may cause issues with certificates or connections. If OWA is supposed to be accessible on the Internet, verify that the external-https zone uses the correct FQDN.


<h1>Logs</h1>


<h3>Retrieve session ID</h3>

Whenever an action URL is navigated to, Office Online creates a unique session ID. Use this session ID to retrieve all server logs related to that session, including information about the WOPI calls that were made to the host. The session ID is passed back in the WOPI action URL HTTP response in the X-UserSessionId response header. It is also passed on every subsequent request made by the browser to Office Online in the X-UserSessionId request header, and it is included in all PostMessages sent from Office Online to the host page in thewdUserSession value. 
The easiest way to retrieve the session ID is to use Fiddler, however, you can also use the request tracking features in the Chrome and Internet Explorer developer tools to capture HTTP requests and determine the value of the X-UserSessionId response header. On detailed instructions how to use Fiddler for retrieving session id, please see this amazing Office Online Documentation Integration  Jump
Depending on the error, the session ID may be presented in the error dialog itself:
  

<h3>Get the logs</h3>

You can find location of the Office Online logs using a cmdlet from officewebapps module: 
(Get-OfficeWebAppsFarm).LogLocation

In SharePoint you can use ULS logs. The default location is C:\Program files\Common Files\Microsoft Shared\Web Server Extensions\14\LOGS for SharePoint 2013 and C:\Program files\Common Files\Microsoft Shared\Web Server Extensions\16\LOGS for SharePoint 2016. ULS Viewer  Jump is a handy tool where you can limit the logs by correlation id, using the session id retrieved earlier.  

<h3>Event Viewer</h3>

Use admin events in the event viewer to see any Office Online-related errors.

<h1>IIS</h1>

In Internet Information Services Manager verify: 
* bindings
* ports 
* certificates
  
For details eview the SSL configuration instructions available at Microsoft Docs Jump . 
* Recycle the app pool 
* Restart the website. 
* Reset IIS with IISreset
 
Image source: 

To verify that Office Online Server is installed and configured correctly, use a web browser to access the Office Online Server discovery URL:
http://servername/hosting/discovery 
The discovery URL is the InternalUrl parameter you specified when you configured your Office Online Server farm, followed by /hosting/discovery. The first few lines should resemble the following example:
<?xml version="1.0" encoding="utf-8" ?>
- <wopi-discovery>
- <net-zone name="internal-http">
- <app name="Excel" favIconUrl="http://servername/x/_layouts/images/FavIcon_Excel.ico Jump " checkLicense="true">
<action name="view" ext="ods" default="true" urlsrc="http://servername/x/_layouts/xlviewerinternal.aspx?< Jump ;ui=UI_LLCC&><rs=DC_LLCC&>" />
<action name="view" ext="xls" default="true" urlsrc="http://servername/x/_layouts/xlviewerinternal.aspx?< Jump ;ui=UI_LLCC&>        <rs=DC_LLCC&>" />
<action name="view" ext="xlsb" default="true"


<h1>Network</h1>

Since Office Online is located on a different machine than server and the client computer using both services, verify that the machines are reachable. 
Firewall. Make sure the firewall allows servers running Office Online Server to initiate HTTP or HTTPS requests to hosts.
Ping IP address and url the OWA server
Make sure all servers in the Office Online Server farm are joined to a domain and are part of the same organizational unit (OU)

<h1>Other</h1>

<h3>EditingEnabled</h3>
If the Office Online documents open, but cannot be edited, check EditingEnabled parameter:
(Get-OfficeWebAppsFarm).EditingEnabled
EditingEnabled parameter enables editing in Office Online when it is used together with SharePoint Server 2016. This parameter isn't used by Skype for Business Server 2015 or Exchange Server because those hosts don't support editing.

<h3>Restart OO</h3>
Open services.msc or use the following cmdlet to restart Office Online 
Restart-Service WACSM

<h3>Check for KB4011024 update</h3> 
The <a href="https://support.microsoft.com/en-us/topic/february-23-2018-update-for-office-online-server-2016-kb4011024-c7142505-b178-e3ec-8556-f0da9d385a8e">KB4011024 update</a> fixes your server status displaying as "Unhealthy" after you install the November update (KB4011020) and January update (KB4011022) in Office Online Server 2016.

<h3>System account</h3>
Make sure you are not using system account. If are, you may receive error similar to: WOPI (GetFile) Proof Data: AccessToken Hash 
A WOPI client must always assume that users have limited permissions to documents. If a host does not set the appropriate user permissions properties, users will not be able to perform operations such as editing documents using a WOPI client. Ultimately, the host has final control over whether WOPI operations attempted by the client should succeed or fail based on the access token provided in the WOPI request. Thus, these properties do not act as an authorization mechanism. Rather, these properties help WOPI clients tailor their UI and behavior to the specific permissions a user has. For example, a WOPI client can hide file renaming UI if the UserCanRename property is false. However, a WOPI client expects that even if that UI were somehow made available to a user without appropriate permissions, the WOPI RenameFile request would fail since the host would determine the action was not permissible based on the access token passed in the request. Note that there is no property that indicates the user has permission to read/view a file. This is because WOPI requires the host to respond to any WOPI request, including CheckFileInfo, with a 401 Unauthorized or 404 Not Found if the access token is invalid or expired.
From:  https://wopi.readthedocs.io/projects/wopirest/en/latest/files/CheckFileInfo.html (fantastic resource - make sure you read it)

<h3>Update-SPWOPIProofKey</h3>
The Update-SPWOPIProofKey cmdlet updates the public key that is used to connect to the WOPI application (Office Online in our case  on the current SharePoint farm. You may want to use this cmdlet if the keys become unsynchronized between the SharePoint farm and the WOPI application. If the keys are unsynchronized, documents may not open in the browser and messages such as "Invalid Proof Signature for file…" or "Invalid Proof Signature for folder..." are found in the Unified Logging System (ULS) logs.

<h3>Reconnect the farm</h3>
Remove-SPWOPIBinding –All:$true
New-SPWOPIBinding -ServerName <WacServerName> -AllowHTTP



<h1>References</h1>
<a href="https://learn.microsoft.com/en-us/officeonlineserver/plan-office-online-server">Plan Office Online Server</a>
Configure Office Online Server for SharePoint Server Jump
Office Online Integration Documentation Jump
CheckFileInfo vs GetFile Jump
Update-SPWOPIProofKey Jump
Social MSDN Jump
