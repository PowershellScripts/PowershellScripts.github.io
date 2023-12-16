---
layout: page
title: 'Add content type using Powershell and CSOM'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2023-12-16'
---


<sup>
Content types were introduced in SharePoint 2007 products and technologies. A content type defines the metadata and behavior for a particular data entity—usually, a business document or item of some kind. Each content type contains references to one or more site columns. You can also associate workflows, information management policies, and document templates with content types. A SharePoint content type pulls together an item and information about the item. It makes it easy to provide consistency across a site. Some of the popular content types are item or a document. A site collection owner can [create custom content types](https://support.microsoft.com/en-us/office/create-or-customize-a-site-content-type-27eb6551-9867-4201-a819-620c5658a60f?ui=en-us&rs=en-us&ad=us) that would reflect more the needs of a company, and add it to selected lists and libraries. A content type can be also [published from a content type hub](https://support.microsoft.com/en-us/office/publish-a-content-type-from-a-content-publishing-hub-58081155-118d-4e7a-9cc5-d43b5dbb7d02?ui=en-us&rs=en-us&ad=us) and managed centrally.
</sup>


<h1>Powershell</h1>

All those actions can be performed through User Interface as well as via Powershell using SharePoint Online SDK or PnP. Below you will find steps for creating, adding, searching, and removing the content types. Full list of all ready, published, and free scripts you will find at the bottom of the article here.

<h1>Add Content Type using CSOM</h1>

As with creating lists or libraries, creating a content type involves using CreationInformation, in this case [ContentTypeCreationInformation class](https://learn.microsoft.com/en-us/previous-versions/office/sharepoint-server/ee539574(v=office.15)?redirectedfrom=MSDN).

```powershell
$lci =New-Object Microsoft.SharePoint.Client.ContentTypeCreationInformation
```

<h3>Properties</h3>

Its properties include Description, Group, ID, Name, ParentContentType, and TypeID which is reserved for internal use and should not be used within the code. Name property is required to be defined before the CreationInformation variable is used for creating the content type.


| Name | Mandatory/Optional | Type |
| -------- | ------- | ------- |
| Description | Optional | System.String |
| Group | Optional | System.String |
| ID | Unique | System.String |
| Name | Mandatory | System.String |
| ParentContentType | Optional | Microsoft.SharePoint.Client.ContentType |
| TypeID | Internal | System.String |


<h5>Group</h5>

Group property specifies a site content type group. Categorizing content types into groups makes it easier for users to find them. It corresponds to the drop down box in the image below and should have a format similar to:
"List Content Types"
"Group Work Content Types"

 

Current content type groups include:
Business Intelligence
Community Content Types
Custom Content Types
Digital Asset Content Types
Display Template Content Types
Document Content Types
Document Set Content Types
Folder Content Types
Group Work Content Types
List Content Types
Page Layout Content Types
Publishing Content Types
Special Content Types

The Group does not decide whether the list content type is available for adding to a specific library or list. E.g.  List Content Type Group is not available for library, because it does not include any compatible content types. If we add a custom content type derived from Document Parent Content Type, the group with a single member (the incompatible ones will be ignored) will appear in the selection:

 
        Remember!                                    
        Under Add Content Types only content types compatible with the list/library are displayed for selection            

If the group is not specified, the content type will be added to Custom Content Types by default.

<h5>Content Type ID</h5>

Content type IDs uniquely identify the content type and are designed to be recursive. The content type ID encapsulates that content type's **lineage**, or the line of parent content types from which the content type inherits. Each content type ID contains the ID of the parent content type, which in turn contains the ID of that content type's parent, and so on, ultimately back to and including the System content type ID. SharePoint uses this information to [determine the relationship between content types](https://learn.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/aa543822(v=office.14)?redirectedfrom=MSDN) and for push-down operations.

You can construct a valid content type ID using one of two conventions:

* Parent content type ID + two hexadecimal values (the two hexadecimal values cannot be "00")
* Parent content type ID + "00" + hexadecimal GUID
 
Source: https://msdn.microsoft.com/en-us/library/office/aa543822%28v=office.14%29.aspx?f=255&MSPPError=-2147217396

An ID of a custom content type based on Item, will look like this when using GUID approach:
0x01 which is an Item Content Type ID
00   
9e862727eed04408b2599b25356e7914
the hexadecimal value 


Item Content Type ID	following the convention	 the hexadecimal value 
0x01	00   	9e862727eed04408b2599b25356e7914
Altogether: 0x01009e862727eed04408b2599b25356e7914
 
You can create a guid automatically using Windows Powershell 
[System.Guid]::NewGuid().toString()
or
[guid]::NewGuid()

You can also invent your own and the following script will be using 0x0100aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa which also works :)

Each content type ID must be unique within a site collection. If the script does not specify the guid, SharePoint will assign one to the content type.



<h5>ParentContentType</h5>

Parent Content Type specifies the parent content type for the content type that will be constructed. The parameter can use both get; and set; methods. ParentContentType parameter is mutually exclusive with the ID parameter. It is due to the information encoded in the ID. ID parameter itself includes the parent content type in its first characters (e.g. "0x01" for the item). So what happens if I will try to create a content type with both those parameters specified?

$lci.ID="0x0108009e862727eed04408b2599b25356e7914"
$lci.ParentContentType=$ctx.Web.ContentTypes.GetById("0x01")

"Parameters.Id, parameters.ParentContentType cannot be used together. Please only use one of them."

 

<h1>Add SharePoint Content Type to a Site Collection</h1>

Script

Having established what information is needed for creating a content type, open Powershell ISE (or a NotePad, that's also an option, though not a very convenient one).

1. Enter paths to SharePoint Online SDK Client and Client.Runtime libraries installed on your local computer.

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

2. Create a Microsoft.SharePoint.Client.ClientContext. $Username, $Url and $AdminPassword variables were defined earlier. ExceuteQuery() is not required at this stage. It is a personal preference and allows me to eliminate incorrect credentials as potential errors at an early stage.

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
$ctx.ExecuteQuery()

3. Define the ContentTypeCreationInformation

$lci =New-Object Microsoft.SharePoint.Client.ContentTypeCreationInformation
  $lci.Description="Description"
  $lci.Name="Powershell Content Type2"
  $lci.ParentContentType=$ctx.Web.ContentTypes.GetById("0x01")
  $lci.Group="List Content Types"

or
$lci =New-Object Microsoft.SharePoint.Client.ContentTypeCreationInformation
#$lci.Description="Description"
$lci.Name="Powershell Content Type2id22aa"
#$lci.Group="List Content Types"
$lci.ID="0x0100aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"

# marks Powershell in-line comment. Non-mandatory parameters Description and Group are commented out here. It is up to you, to define and use them or rely on default settings.

4. Using the CreationInformation, add the content type to site content types.
$ContentType = $ctx.Web.ContentTypes.Add($lci)
$ctx.Load($contentType)
$ctx.ExecuteQuery()

5. Error handling:

$ContentType = $ctx.Web.ContentTypes.Add($lci)
$ctx.Load($contentType)
try
   {
      
       $ctx.ExecuteQuery()
       Write-Host "Content Type " $Title " has been added to " $Url
   }
   catch [Net.WebException]
   {
      Write-Host $_.Exception.ToString()
   }

<h2>Full script</h2>

```powershell
  # Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
 
# Insert the credentials and the name of the admin site
$Username="admin@tenant.onmicrosoft.com"
$AdminPassword=Read-Host -Prompt "Password" -AsSecureString
$AdminUrl="https://tenant.sharepoint.com/sites/teamsitewithlibraries Jump "
 
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url) 
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
 
$ctx.ExecuteQuery()
 
$lci =New-Object Microsoft.SharePoint.Client.ContentTypeCreationInformation
$lci.Description="Description"
$lci.Name="Powershell Content Type2"
$lci.ParentContentType=$ctx.Web.ContentTypes.GetById("0x01")
$lci.Group="List Content Types"
   
$ContentType = $ctx.Web.ContentTypes.Add($lci)
$ctx.Load($contentType)
  
  try{
         $ctx.ExecuteQuery()
         Write-Host "Content Type " $Title " has been added to " $Url
     }
  catch()
     {
        Write-Host $_.Exception.ToString()
     }
 
```powershell 


The script can also be downloaded from Github.


<h1>Add SharePoint Content Type to Content Hub</h1>

Content hub is a feature in SharePoint Online to manage content types centrally and publish them to the subscribed sites. It provides a central location for unified management of content types. From the content type hub you can publish a given content type across multiple site collections. The central location ensures that the settings, columns, and look of the content type is consistent across the whole organization. The content types appear in the subscribing sites under Site Settings>Content Type Publishing:

 

To compare, in the previous example, having added a content type to a site collection scope, you can propagate it only to underlying lists. Adding the content type to a content type hub will allow you to use the same content type across your whole tenant.

How to find content hub?

From site collection level
Select the Options button Settings button and then select Site Settings.

Under Site Collection Administration, click Content type publishing.

In the Hubs section, you can see the names of any Managed Metadata Service applications that publish content types to this site collection listed in bold text. After the service application names, you can see the URLs for the hub sites. You can also see a list of the subscribed content types.

Source: https://support.office.com/en-za/article/Publish-a-content-type-from-a-content-publishing-hub-58081155-118d-4e7a-9cc5-d43b5dbb7d02 Jump

Direct url

https://TENANT.sharepoint.com/sites/contenttypehub

where TENANT is the name of your SharePoint Online tenant.

Script

The ContentTypeCreationInformation and its properties undergo the same considerations as when adding it to a site collection level. As can be seen by the url above, in many respects the content type hub is just another site collection. The full script adding a content type to CTH differs only through its url:

function New-SPOContentType
{
param(
[Parameter(Mandatory=$true,Position=1)]
        [string]$Username,
        [Parameter(Mandatory=$true,Position=2)]
        $AdminPassword,
        [Parameter(Mandatory=$true,Position=3)]
        [string]$Url,
[Parameter(Mandatory=$true,Position=4)]
        [string]$Description,
[Parameter(Mandatory=$true,Position=5)]
        [string]$Name,
[Parameter(Mandatory=$true,Position=6)]
        [string]$Group,
[Parameter(Mandatory=$true,Position=7)]
        [string]$ParentContentTypeID
 
        )
   
  $ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
  $ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
 
  $ctx.ExecuteQuery()
 
   
 
  $lci =New-Object Microsoft.SharePoint.Client.ContentTypeCreationInformation
  $lci.Description=$Description
  $lci.Name=$Name
  #$lci.ID="0x0108009e862727eed04408b2599b25356e7914"
  $lci.ParentContentType=$ctx.Web.ContentTypes.GetById($ParentContentTypeID)
  $lci.Group=$Group
   
  $ContentType = $ctx.Web.ContentTypes.Add($lci)
  $ctx.Load($contentType)
  try
     {
        
         $ctx.ExecuteQuery()
         Write-Host "Content Type " $Name " has been added to " $Url
     }
     catch [Net.WebException]
     {
        Write-Host $_.Exception.ToString()
     }
 
      
 
}
 
 
 
  # Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
 
# Insert the credentials and the name of the admin site
$Username="admin@tenant.onmicrosoft.com"
$AdminPassword=Read-Host -Prompt "Password" -AsSecureString
$AdminUrl="https://tenant.sharepoint.com/sites/contenttypehub Jump "
$Description="desc"
$Name="From Powershell directly to CTH"
$ParentContentTypeID="0x01"
$Group="List Content Types"
 
 
New-SPOContentType -Username $Username -AdminPassword $AdminPassword -Url $AdminUrl -Description $Description -Name $Name -Group $Group -ParentContentTypeID $ParentContentTypeID

The script can be downloaded from Technet Gallery here Jump .


Directly to list

A content type can be created directly within a list content types and be available only in a this list's scope. It is a practical solution if the content type you are going to create is very specific only to this particular list and should not be used elsewhere.

Content Type Management

In order to see the added content types and be able to use them, ensure that the list has content type management enabled. 
This setting can also be changed via Powershell script and examples can be found in the scripts below:
Allow content type management for all lists in site collection using Powershell Jump
Allow content type management for all lists in a site using Powershell Jump
Set content type management setting for SharePoint Online list using Powershell Jump
Custom Powershell cmdlet Set-SPOList -ContentTypesEnabled Jump

Script

When it comes to adding a content type to list, there are several options, depending on the requirements. Using one script the content type can be added to:
a single list
all lists
several lists fulfilling common criteria
     A single list

In order to target the list, some information about the list is needed. For one specific list, the best are ID or Title. They allow to leverage GetByID Jump  andGetByTitle Jump  methods:

$ctx.Web.Lists.GetByID("4C834F99-D908-4E1C-A04F-C8B1F971371C")
or
$ctx.Web.Lists.GetByTitle("The Title of My List")

     All Lists

It is also possible to add the content type in all existing lists. In order to add the content type to all lists, you need to retrieve the list collection from the .Web and loop through each of the lists, adding the content type there 

$ctx.Load($ctx.Web.Lists)
  $ctx.ExecuteQuery()
  foreach($ll in $ctx.Web.Lists)
  {
    #Add Content Type here
  }
     
      Some Lists

 Once you have created your content type, you may decide that it is appropriate only for specific lists. There is no end to filter options. You can choose only announcement lists, only lists with content type enabled, lists with a certain content type already added, lists created after or before a certain date, lists with enabled versioning, or those that include certain fields or views. Below there are some of the examples:  
tasks lists
foreach($ll in $ctx.Web.Lists)
  {
 
   if($ll.BaseTemplate -eq 107 -or $ll.BaseTemplate -eq 171)
     {
          # Add Content Type comes here
     }
  }

lists with workflows
$ctx.Load($ctx.Web.Lists)
$ctx.ExecuteQuery()
 
  foreach($ll in $ctx.Web.Lists)
  {
 
     $ctx.Load($ll.WorkflowAssociations)
     $ctx.ExecuteQuery()
 
      if($ll.WorkflowAssociations.Count -gt 0)
       {
             # Add Content Type comes here
      
        }
  }

Full script

The script below is an example of adding a content type to lists with workflows. In order to get a script for other scenarios, substitute its sections with appropriate modifications. The scripts can be downloaded from Technet Gallery:
Add Content Type to Lists with Workflows  Jump
Add Content Type to Task Lists Jump
Create content type and add it to all lists in one site Jump
Create content type and add directly to SPO list using Powershell Jump

function New-SPOContentType
{
param(
[Parameter(Mandatory=$true,Position=1)]
        [string]$Username,
        [Parameter(Mandatory=$true,Position=2)]
        $AdminPassword,
        [Parameter(Mandatory=$true,Position=3)]
        [string]$Url,
[Parameter(Mandatory=$true,Position=4)]
        [string]$Description,
[Parameter(Mandatory=$true,Position=5)]
        [string]$Name,
[Parameter(Mandatory=$true,Position=6)]
        [string]$Group,
[Parameter(Mandatory=$true,Position=7)]
        [string]$ParentContentTypeID
 
        )
   
  $ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
  $ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
  $ctx.Load($ctx.Web.Lists)
  $ctx.ExecuteQuery()
 
   
 
  $lci =New-Object Microsoft.SharePoint.Client.ContentTypeCreationInformation
  $lci.Description=$Description
  $lci.Name=$Name
  #$lci.ID="0x0100aa862727aed04408b2599b25356e7000"
  $lci.ParentContentType=$ctx.Web.ContentTypes.GetById($ParentContentTypeID)
  $lci.Group=$Group
   
  foreach($ll in $ctx.Web.Lists)
  {
 
     $ctx.Load($ll.WorkflowAssociations)
     $ctx.ExecuteQuery()
 
   if($ll.WorkflowAssociations.Count -gt 0)
   {
  $ContentType = $ll.ContentTypes.Add($lci)
  $ctx.Load($contentType)
  try
     {
        
         $ctx.ExecuteQuery()
         Write-Host "Adding content type " $Name " to " $ll.Title
     }
     catch [Net.WebException]
     {
        Write-Host $_.Exception.ToString()
     }
 
      
     }
     }
}
 
 
 
  # Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
 
# Insert the credentials and the name of the admin site
$Username="admin@tenant.onmicrosoft.com"
$AdminPassword=Read-Host -Prompt "Password" -AsSecureString
$AdminUrl="https://tenant.sharepoint.com/sites/teamsitewithlists Jump "
$Description="desc"
$Name="From PS to Tasks234"
$ParentContentTypeID="0x0107"
$Group="List Content Types"
 
 
 
New-SPOContentType -Username $Username -AdminPassword $AdminPassword -Url $AdminUrl -Description $Description -Name $Name -Group $Group -ParentContentTypeID$ParentContentTypeID

From an existing content type

Once you have added a content type to a site collection or to a Content Type Hub and it has propagated to subscribing sites, it can be also added to a list as an existing content type. Such solution allows centralized control over the settings of the content type. You can apply changes at a site collection level and then update all content types:

 

or you can edit the content type in the Content Type Hub and republish it in order to propagate the changes to all subscribing sites. The changes may take up to 48 hours Jump . 

 


Script

There is a designated method for adding a content type which already exists at a site collection level. The method is called AddExistingContentType Jump  and belongs toContentTypeCollection class Jump .  The method returns a ContentType Jump  object. 

1. Create a context.

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
$ctx.Load($ctx.Web.Lists)
$ctx.ExecuteQuery()

2. Get the Content type you want to add using its ID.

$contentType=$ctx.Web.ContentTypes.GetById($ContentTypeID)
$ctx.Load($contentType)

3. Retrieve the list and the content type collection associated with that list:

$ll=$ctx.Web.Lists.GetByTitle($ListTitle)
$ctx.load($ll)
$ctx.load($ll.ContentTypes)
$ctx.ExecuteQuery()

4. Make sure the Content Type Management is allowed.

$ll.ContentTypesEnabled=$true

5. Add the content type.  

$AddedContentType=$ll.ContentTypes.AddExistingContentType($contentType)
$ll.Update()

The method returns a content type, so running the method alone 

$ll.ContentTypes.AddExistingContentType($contentType)

will return "Collection has not been initialized" error as described brilliantly here. Jump


6. Execute the request and add basic error handling.

try
     {
        
         $ctx.ExecuteQuery()
         Write-Host "Adding content type " $AddedContentType.Name " to " $ll.Title
     }
catch [Net.WebException]
     {
        Write-Host $_.Exception.ToString()
     }

Full script

 The above described commands have been packaged into a function and script available for download here: Add existing content type directly to SPO list using Powershell Jump

function Add-SPOContentType
{
param(
[Parameter(Mandatory=$true,Position=1)]
        [string]$Username,
        [Parameter(Mandatory=$true,Position=2)]
        $AdminPassword,
        [Parameter(Mandatory=$true,Position=3)]
        [string]$Url,
        [Parameter(Mandatory=$true,Position=4)]
        [string]$ListTitle,
[Parameter(Mandatory=$true,Position=7)]
        [string]$ContentTypeID
 
        )
   
  $ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
  $ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
  $ctx.Load($ctx.Web.Lists)
  $ctx.ExecuteQuery()
 
  $contentType=$ctx.Web.ContentTypes.GetById($ContentTypeID)
  $ctx.Load($contentType)
 
 $ll=$ctx.Web.Lists.GetByTitle($ListTitle)
 $ctx.load($ll)
 $ctx.load($ll.ContentTypes)
 $ctx.ExecuteQuery()
  $ll.ContentTypesEnabled=$true
 $AddedContentType=$ll.ContentTypes.AddExistingContentType($contentType)
 $ll.Update()
  try
     {
        
         $ctx.ExecuteQuery()
         Write-Host "Adding content type " $AddedContentType.Name " to " $ll.Title
     }
     catch [Net.WebException]
     {
        Write-Host $_.Exception.ToString()
     }
 
      
      
}
 
 
 
  # Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
 
# Insert the credentials and the name of the admin site
$Username="admin@tenant.onmicrosoft.com"
$AdminPassword=Read-Host -Prompt "Password" -AsSecureString
$AdminUrl="https://tenant.sharepoint.com/sites/teamsitewithlists Jump "
$ListTitle="tas1207"
$ContentTypeID="0x01200200C44754774BD8D4449F4B7E3FE70A7E0E"
 
 
 
Add-SPOContentType -Username $Username -AdminPassword $AdminPassword -Url $AdminUrl -ListTitle $ListTitle -ContentTypeID $ContentTypeID
