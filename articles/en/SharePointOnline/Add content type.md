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

All those actions can be performed through User Interface as well as via Powershell using SharePoint Online SDK or PnP. Below you will find steps for adding SharePoint content type to a site, a content type hub, and a SharePoint list. Full list of all ready, published, and free scripts is at the bottom of the article here.

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

 <img src="/articles/images/Github-AddContentType1.png" width="400"><br/>

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
<img src="/articles/images/Github-AddContentType2.png" width="400"><br/>
 
        Remember!                                    
        Under Add Content Types only content types compatible with the list/library are displayed for selection            

If the group is not specified, the content type will be added to Custom Content Types by default.

<h5>Content Type ID</h5>

Content type IDs uniquely identify the content type and are designed to be recursive. The content type ID encapsulates that content type's **lineage**, or the line of parent content types from which the content type inherits. Each content type ID contains the ID of the parent content type, which in turn contains the ID of that content type's parent, and so on, ultimately back to and including the System content type ID. SharePoint uses this information to [determine the relationship between content types](https://learn.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/aa543822(v=office.14)?redirectedfrom=MSDN) and for push-down operations.

You can construct a valid content type ID using one of two conventions:

* Parent content type ID + two hexadecimal values (the two hexadecimal values cannot be "00")
* Parent content type ID + "00" + hexadecimal GUID

 <img src="/articles/images/Github-AddContentType2-1.png" width="400"><br/>
<sup>Source: https://msdn.microsoft.com/en-us/library/office/aa543822%28v=office.14%29.aspx?f=255&MSPPError=-2147217396</sup>



An ID of a custom content type based on Item, will look like this when using GUID approach:
0x01 which is an Item Content Type ID
00   
9e862727eed04408b2599b25356e7914
the hexadecimal value 


Item Content Type ID	following the convention	 the hexadecimal value 
0x01	00   	9e862727eed04408b2599b25356e7914
Altogether: 0x01009e862727eed04408b2599b25356e7914
 
You can create a guid automatically using Windows Powershell 
```powershell
[System.Guid]::NewGuid().toString()
```
or
```powershell
[guid]::NewGuid()
```
You can also invent your own and the following script will be using 0x0100aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa which also works :)

Each content type ID must be unique within a site collection. If the script does not specify the guid, SharePoint will assign one to the content type, based on the parent content type.



<h5>ParentContentType</h5>

Parent Content Type specifies the parent content type for the content type that will be constructed. The parameter can use both get; and set; methods. ParentContentType parameter is mutually exclusive with the ID parameter. It is due to the information encoded in the ID. ID parameter itself includes the parent content type in its first characters (e.g. "0x01" for the item). So what happens if I will try to create a content type with both those parameters specified?

```powershell
$lci.ID="0x0108009e862727eed04408b2599b25356e7914"
$lci.ParentContentType=$ctx.Web.ContentTypes.GetById("0x01")
```
"Parameters.Id, parameters.ParentContentType cannot be used together. Please only use one of them."


 <img src="/articles/images/Github-AddContentType3.png" width="400"><br/>
 

<h1>Add SharePoint Content Type to a Site Collection</h1>

<h3>Script</h3>

Having established what information is needed for creating a content type, open Powershell ISE (or a NotePad, that's also an option, though not a very convenient one).

1. Enter paths to SharePoint Online SDK Client and Client.Runtime libraries installed on your local computer.

```powershell
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
```

2. Create a Microsoft.SharePoint.Client.ClientContext. $Username, $Url and $AdminPassword variables were defined earlier. ExceuteQuery() is not required at this stage. It is a personal preference and allows me to eliminate incorrect credentials as potential errors at an early stage.

```powershell
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
$ctx.ExecuteQuery()
```

3. Define the ContentTypeCreationInformation

```powershell
  $lci =New-Object Microsoft.SharePoint.Client.ContentTypeCreationInformation
  $lci.Description="Description"
  $lci.Name="Powershell Content Type2"
  $lci.ParentContentType=$ctx.Web.ContentTypes.GetById("0x01")
  $lci.Group="List Content Types"
```

or

```powershell
$lci =New-Object Microsoft.SharePoint.Client.ContentTypeCreationInformation
#$lci.Description="Description"
$lci.Name="Powershell Content Type2id22aa"
#$lci.Group="List Content Types"
$lci.ID="0x0100aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
```

`# marks Powershell in-line comment. Non-mandatory parameters Description and Group are commented out here. It is up to you, to define and use them or rely on default settings.

4. Using the CreationInformation, add the content type to site content types.

```powershell
$ContentType = $ctx.Web.ContentTypes.Add($lci)
$ctx.Load($contentType)
$ctx.ExecuteQuery()
```

5. Error handling:

```powershell
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
```

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
 
```


The script can also be downloaded from Github.


<h1>Add SharePoint Content Type to Content Hub</h1>

Content hub is a feature in SharePoint Online to manage content types centrally and publish them to the subscribed sites. It provides a central location for unified management of content types. From the content type hub you can publish a given content type across multiple site collections. The central location ensures that the settings, columns, and look of the SharePoint content type are consistent across the whole organization. The content types appear in the subscribing sites under Site Settings>Content Type Publishing:
 <img src="/articles/images/Github-AddContentType4.png" width="400"><br/>
 

To compare, in the previous example, after adding a content type to a site collection scope, you can propagate it only to underlying lists. Adding the content type to a content type hub will allow you to use the same content type across your whole tenant.

<h3>How to find content hub?</h3>

* From site collection level
    * Select the Options button Settings button and then select Site Settings.

    * Under Site Collection Administration, click Content type publishing.

In the Hubs section, you can see the names of any Managed Metadata Service applications that publish content types to this site collection listed in bold text. After the service application names, you can see the URLs for the hub sites. You can also see a list of the subscribed content types.

Source: https://support.office.com/en-za/article/Publish-a-content-type-from-a-content-publishing-hub-58081155-118d-4e7a-9cc5-d43b5dbb7d02

* Direct url

```powershell
https://TENANT.sharepoint.com/sites/contenttypehub
```
where TENANT is the name of your SharePoint Online tenant.

<h3>Script</h3>

The ContentTypeCreationInformation and its properties undergo the same considerations as when adding it to a site collection level. As can be seen by the url above, in many respects the content type hub is just another site collection. The full script adding a content type to CTH differs only through its url:

```powershell
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
  
try{    
         $ctx.ExecuteQuery()
         Write-Host "Content Type " $Name " has been added to " $Url
     }
catch()
     {
        Write-Host $_.Exception.ToString()
     }
 
 ```  

The script can be downloaded from Github.

<br/><br/>

<h1>Directly to list</h1>

A content type can be created directly to list content types and be available only in this list's scope. It is a practical solution if the content type you are going to create is very specific only to this particular list and should not be used elsewhere.

<h3>Content Type Management</h3>

In order to see the added content types and be able to use them, ensure that the list has content type management enabled. 
This setting can also be changed via Powershell script and examples can be found in the scripts below:
* [Allow content type management for all lists in site collection using Powershell](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/0329e8e1b21de995652863311b53306749908550/Content%20Types/Content%20Types%20Management%20Setting/Allow%20content%20type%20management%20for%20all%20lists%20in%20site%20collection)
* [Allow content type management for all lists in a site using Powershell](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/0329e8e1b21de995652863311b53306749908550/Content%20Types/Content%20Types%20Management%20Setting/Allow%20content%20type%20management%20for%20all%20lists%20in%20a%20site)
* [Set content type management setting for SharePoint Online list using Powershell](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/blob/3aa558029abcea8ec1c24c0cfecded8abf25339a/Content%20Types/Content%20Types%20Management%20Setting/Set%20content%20type%20management%20setting%20for%20a%20single%20list/ReadMe.md)


<h3>Script</h3>

When it comes to adding a content type to list, there are several options, depending on the requirements. Using one script the content type can be added to:
* a single list
* all lists
* several lists fulfilling common criteria

  <h4>A single list</h4>

In order to target the list, some information about the list is needed. For one specific list, the best are ID or Title. They allow to leverage GetByID and GetByTitle methods:

```powershell
$ctx.Web.Lists.GetByID("4C834F99-D908-4E1C-A04F-C8B1F971371C")
```

or

```powershell
$ctx.Web.Lists.GetByTitle("The Title of My List")
```


<h4>All Lists</h4>

It is also possible to add the content type in all existing lists. In order to add the content type to all lists, you need to retrieve the list collection from the .Web and loop through each of the lists, adding the content type there 

```powershell
$ctx.Load($ctx.Web.Lists)
  $ctx.ExecuteQuery()
  foreach($ll in $ctx.Web.Lists)
  {
    #Add Content Type here
  }
 ```
    
<h4>Some Lists</h4>

 Once you have created your content type, you may decide that it is appropriate only for specific lists. There is no end to filter options. You can choose only announcement lists, only lists with content type enabled, lists with a certain content type already added, lists created after or before a certain date, lists with enabled versioning, or those that include certain fields or views. Below there are some of the examples:  

* tasks lists

```powershell
foreach($ll in $ctx.Web.Lists)
  {
 
   if($ll.BaseTemplate -eq 107 -or $ll.BaseTemplate -eq 171)
     {
          # Add Content Type comes here
     }
  }
```

* lists with workflows

```powershell
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
```


<h3>Full script</h3>

The script below is an example of adding a content type to lists with workflows. In order to get a script for other scenarios, substitute its sections with appropriate modifications. The scripts can be downloaded from Technet Gallery:
Add Content Type to Lists with Workflows  Jump
Add Content Type to Task Lists Jump
Create content type and add it to all lists in one site Jump
Create content type and add directly to SPO list using Powershell Jump


```powershell
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
 
     if($ll.WorkflowAssociations.Count -gt 0){
         $ContentType = $ll.ContentTypes.Add($lci)
         $ctx.Load($contentType)
  
         try{
            $ctx.ExecuteQuery()
            Write-Host "Adding content type " $Name " to " $ll.Title
          }
         catch [Net.WebException]
          {
            Write-Host $_.Exception.ToString()
          }

     }
   }
 
``` 



<h2>From an existing content type</h2>

Once you have added a content type to a site collection or to a Content Type Hub and it has propagated to subscribing sites, it can be also added to a list as an existing content type. Such solution allows centralized control over the settings of the content type. You can apply changes at a site collection level and then update all content types:
 <img src="/articles/images/Github-AddContentType5.png" width="400"><br/>
 

or you can edit the content type in the Content Type Hub and republish it in order to propagate the changes to all subscribing sites. The changes may take up to 48 hours. 

  <img src="/articles/images/Github-AddContentType6.png" width="400"><br/>


<h3>Script</h3>

There is a designated method for adding a content type which already exists at a site collection level. The method is called AddExistingContentType  and belongs toContentTypeCollection class.  The method returns a ContentType object. 

1. Create a context.

```powershell
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
$ctx.Load($ctx.Web.Lists)
$ctx.ExecuteQuery()
```

2. Get the Content type you want to add using its ID.

```powershell
$contentType=$ctx.Web.ContentTypes.GetById($ContentTypeID)
$ctx.Load($contentType)
```

3. Retrieve the list and the content type collection associated with that list:

```powershell
$ll=$ctx.Web.Lists.GetByTitle($ListTitle)
$ctx.load($ll)
$ctx.load($ll.ContentTypes)
$ctx.ExecuteQuery()
```

4. Make sure the Content Type Management is allowed.

```powershell
$ll.ContentTypesEnabled=$true
```

5. Add the content type.  

```powershell
$AddedContentType=$ll.ContentTypes.AddExistingContentType($contentType)
$ll.Update()
```
The method returns a content type, so running the method alone 

```powershell
$ll.ContentTypes.AddExistingContentType($contentType)
```

will return "Collection has not been initialized" error, as described brilliantly [by Mikael Svenson here.](https://www.techmikael.com/2014/04/adding-content-type-to-list-using-csom.html)


6. Execute the request and add basic error handling.

```powershell
try
     {
        
         $ctx.ExecuteQuery()
         Write-Host "Adding content type " $AddedContentType.Name " to " $ll.Title
     }
catch [Net.WebException]
     {
        Write-Host $_.Exception.ToString()
     }
```


<h3>Full script</h3>

 The above described commands have been packaged into a function and script available for download here: Add existing content type directly to SPO list using Powershell.


```powershell
  # Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
 
# Insert the credentials and the name of the admin site
$Username="admin@tenant.onmicrosoft.com"
$AdminPassword=Read-Host -Prompt "Password" -AsSecureString
$AdminUrl="https://tenant.sharepoint.com/sites/teamsitewithlists Jump "
$ListTitle="tas1207"
$ContentTypeID="0x01200200C44754774BD8D4449F4B7E3FE70A7E0E"
   
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
 
```




<br/><br/>

## See Also


[Enable content type management](https://powershellscripts.github.io/articles/en/spo/enablect.md)




<!-- Default Statcounter code for Content Types add
https://powershellscripts.github.io/articles/en/SharePointOnline/Add%20content%20type/
-->
<script type="text/javascript">
var sc_project=12953202; 
var sc_invisible=0; 
var sc_security="fe97b4fc"; 
var sc_client_storage="disabled"; 
var scJsHost = "https://";
document.write("<sc"+"ript type='text/javascript' src='" +
scJsHost+
"statcounter.com/counter/counter.js'></"+"script>");
</script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/12953202/0/fe97b4fc/0/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
