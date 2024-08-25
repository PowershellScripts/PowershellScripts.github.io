---
layout: page
title: 'Find content type ID using Powershell'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-08-18'
---

# Intro

Content types in SharePoint are vital for structuring and managing content across sites and libraries. Each content type is associated with a unique Content Type ID, which is necessary for various administrative tasks, including scripting and automation. This article provides a straightforward guide on how to find a Content Type ID using PowerShell.

<br/>

# PnP Powershell
The easiest way to prgrammatically get SharePoint content type ID, is through PnP Powershell. You need to get the content type first, then its ID.

<br/>

### From a list



##### Option 1
Get all content types, a list of their names and Ids.

```powershell
# Connect to SharePoint Online
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Specify the list name
$listName = "Your List Name"

# Get all content types from the list
$contentTypes = Get-PnPContentType -List $listName

# Output content type names and IDs
$contentTypes | ForEach-Object { Write-Host "Content Type: $($_.Name), ID: $($_.Id.StringValue)" }

# Disconnect from SharePoint Online
Disconnect-PnPOnline
```


##### Option 2
Get all content types, a list of their names and Ids.

```powershell
# Connect to SharePoint Online
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Specify the list name
$listName = "Your List Name"

# Get all content types from the list
$contentTypes = Get-PnPContentType -List $listName

# Output content type names and IDs
$contentTypes | select name, Id

# Disconnect from SharePoint Online
Disconnect-PnPOnline
```


##### Option 3
Get all content types, a list of their names and Ids.

```powershell
# Connect to SharePoint Online
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Specify the list name
$listName = "Your List Name"

# Get all content types from the list
Get-PnPContentType -List $listName | select name, Id

# Disconnect from SharePoint Online
Disconnect-PnPOnline
```



##### Option 4
Get the ID of a single content type in SharePoint list.


```powershell
# Connect to SharePoint Online
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Specify the name of the content type you want to find
$contentTypeName = "Your Content Type Name"

# Get the content type from the site
$contentType = Get-PnPContentType -Identity $contentTypeName -List $listName

# Output the Content Type ID
Write-Host "Content Type ID: $($contentType.Id.StringValue)"

# Disconnect from SharePoint Online
Disconnect-PnPOnline
```



#### From a site


##### Option 1
Get all content types from a SharePoint site, and list their names and Ids.

```powershell
# Connect to SharePoint Online
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Get all content types from the list
$contentTypes = Get-PnPContentType

# Output content type names and IDs
$contentTypes | ForEach-Object { Write-Host "Content Type: $($_.Name), ID: $($_.Id.StringValue)" }

# Disconnect from SharePoint Online
Disconnect-PnPOnline
```


##### Option 2
Get all content types from a SharePoint site, and list their names and Ids.

```powershell
# Connect to SharePoint Online
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Get all content types from the list
$contentTypes = Get-PnPContentType

# Output content type names and IDs
$contentTypes | select name, Id

# Disconnect from SharePoint Online
Disconnect-PnPOnline
```


##### Option 3
Get all content types from a SharePoint site, and list their names and Ids.

```powershell
# Connect to SharePoint Online
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Get all content types from the list
Get-PnPContentType | select name, Id

# Disconnect from SharePoint Online
Disconnect-PnPOnline
```



##### Option 4
Get the ID of a single content type in SharePoint list.


```powershell
# Connect to SharePoint Online
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Specify the name of the content type you want to find
$contentTypeName = "Your Content Type Name"

# Get the content type from the site
$contentType = Get-PnPContentType -Identity $contentTypeName

# Output the Content Type ID
Write-Host "Content Type ID: $($contentType.Id.StringValue)"

# Disconnect from SharePoint Online
Disconnect-PnPOnline
```

<br/>

# Using REST API
You can also retrieve Content Type ID using Powershell and REST API. I am using here PnP to get access token, but there are other ways.

```powershell
# Set variables
$siteUrl = "https://yourtenant.sharepoint.com/sites/yoursite"
$contentTypeName = "Your Content Type Name"
$restUrl = "$siteUrl/_api/web/contenttypes?$filter=Name eq '$($contentTypeName)'"

# Get the access token using PnP PowerShell
$token = Get-PnPAccessToken

# Make the REST API request
$response = Invoke-RestMethod -Uri $restUrl -Headers @{
    "Authorization" = "Bearer $token"
    "Accept"        = "application/json; odata=verbose"
} -Method Get

# Check if content type was found and output the ID
if ($response.d.results.Count -gt 0) {
    $contentTypeId = $response.d.results[0].Id.StringValue
    Write-Host "Content Type ID: $contentTypeId"
} else {
    Write-Host "Content Type not found"
}
```

<br/>

# PowerShell scripting with CSOM

You van also get SharePoint content type ID using CSOM and Powershell: 

1. Download and install SharePoint Online SDK.
2. Open Powershell ISE.
3. Add the SDK libraries:


```powershell
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
```

4. Save the following information into variables. Bear in mind that you are connecting to any given SharePoint site or subsite, and -admin is not required in the URL:

```powershell
$Username="t@trial876.onmicrosoft.com"
$Password=Read-Host -Prompt "Password" -AsSecureString
$Url="https://trial876.sharepoint.com"
```

5. Create context:

```powershell
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $Password)
$ctx.ExecuteQuery()
```

6. Load the site and its content types:

```powershell
$ctx.Load($ctx.Web)
$ctx.Load($ctx.Web.ContentTypes)
$ctx.ExecuteQuery()
```

7. You now have all the necessary data. It’s up to you and your specific requirements to decide what you want to display next.

All content types and their IDs:

```powershell
foreach($cc in $ctx.Web.ContentTypes)
{
   Write-Host $cc.Name "  ID: " $cc.ID
     
}
```

Content type by a given name:

```powershell
foreach($cc in $ctx.Web.ContentTypes)
{
   if($cc.Name -eq "East Asia Contact")
   {
      Write-Host $cc.Id
   }
     
}
```


<br/>

# PowerShell SPOMod
An old favourite of mine, slowly getting deprecated, but I am mentioning it here out of sentiment.

### Prerequisites

1. Download and install SharePoint Online SDK.
2. Download SPOMod available here.
3. Open SPOMod current file (in ISE or NotePad) and scroll down to the following lines:

```powershell
# Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
```

4. Make sure that the paths correspond to the locations of SDK on your machine. Most often you need to change 16 into 15.
5. Save the file.
6. Open PowerShell as an Administrator.
7. Run Import-Module PathToTheSPOModFile
8. Run Connect-SPOCSOM cmdlet.

### List Content Type

To see all properties of the content type, enter:

```powershell
Get-SPOContentType -ListTitle MyListTitle
```
 

To see only IDs, enter:

```powershell
Get-SPOContentType -ListTitle ghgh | select name, id
```
 


### Site Content Type

To see all content types with all their properties, enter:

```powershell
Get-SPOContentType
```

To see only content types and their respective ids:

```powershell
Get-SPOContentType | select name, id
```

You can add | ft -autosize  to see untruncated results in the Shell:

```powershell
Get-SPOContentType | select name, id | ft -AutoSize
```
 

### From a subsite

```powershell
Get-SPOContentType -Available | select name, id | ft -Autosize
```
 



### By name

```powershell
Get-SPOContentType | where {$_.Name -eq "Task"} | select id
```
 




# See Also

[Get content type ID using User Interface](https://powershellscripts.github.io/articles/en/SharePointOnline/findctID/)
