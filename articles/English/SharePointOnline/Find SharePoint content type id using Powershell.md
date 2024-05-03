PowerShell scripting with CSOM

There are no out-of-the-box cmdlets for SharePoint Online. One of the options is CSOM: 

1. Download and install SharePoint Online SDK.
2. Open Powershell ISE.
3. Add the SDK libraries:

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

4. Save the following information into variables. Bear in mind that you are connecting to any given SharePoint site or subsite, and -admin is not required in the URL:

$Username="t@trial876.onmicrosoft.com"
$Password=Read-Host -Prompt "Password" -AsSecureString
$Url="https://trial876.sharepoint.com"

5. Create context:

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $Password)
$ctx.ExecuteQuery()

6. Load the site and its content types:

$ctx.Load($ctx.Web)
$ctx.Load($ctx.Web.ContentTypes)
$ctx.ExecuteQuery()

7. Now you have all the data. Now it depends on you and your requirements what you want to display next.

All content types and their IDs:
foreach($cc in $ctx.Web.ContentTypes)
{
   Write-Host $cc.Name "  ID: " $cc.ID
     
}


Content type by a given name:
foreach($cc in $ctx.Web.ContentTypes)
{
   if($cc.Name -eq "East Asia Contact")
   {
      Write-Host $cc.Id
   }
     
}
PowerShell SPOMod
Prerequisites
1. Download and install SharePoint Online SDK.
2. Download SPOMod available here.
3. Open SPOMod current file (in ISE or NotePad) and scroll down to the following lines:
# Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

4. Make sure that the paths correspond to the locations of SDK on your machine. Most often you need to change 16 into 15.
5. Save the file.
6. Open PowerShell as an Administrator.
7. Run Import-Module PathToTheSPOModFile
8. Run Connect-SPOCSOM cmdlet.

List Content Type

To see all properties of the content type, enter:
Get-SPOContentType -ListTitle MyListTitle

 

To see only IDs, enter:
Get-SPOContentType -ListTitle ghgh | select name, id

 


Site Content Type

To see all content types with all their properties, enter:
Get-SPOContentType

To see only content types and their respective ids:
Get-SPOContentType | select name, id

You can add | ft -autosize  to see untruncated results in the Shell:
Get-SPOContentType | select name, id | ft -AutoSize

 

From a subsite

Get-SPOContentType -Available | select name, id | ft -Autosize

 



By name
Get-SPOContentType | where {$_.Name -eq "Task"} | select id

 

