---
layout: page
title: 'SharePoint content types in Powershell: known errors'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2023-12-26'
---

<sup>The errors below are based on THOUSANDS of tests performed on various Office 365 tenants. That by no means makes them definite, final or in any way exclusive. Bear in mind that the same error messages may have several causes and treat the descriptions below as suggestions for troubleshooting, not a definite and only possible cause. </sup>


### Updating SharePoint content types

When you update the content type remember to specify whether the child content types should be updated as well.



* If you do not specify the Boolean value, an error will occur **Cannot find an overload for "Update" and the argument count: "0".**

:x: ```$ContentType.Update()```

:heavy_check_mark:  ```$ContentType.Update($false) ```    :heavy_check_mark:  ```$ContentType.Update($true) ```

<br/>

* **Exception calling "ExecuteQuery" with "0" argument(s): "The content type has no children."** If you set Update to $true, but the content type doesn't have any child content types. This can happen with e.g. a list content type  

:x: ```$ContentType.Update($true)```

:heavy_check_mark:  ```$ContentType.Update($false)```

     
  <br/><br/>

### Loading SharePoint content types

* **Property 'Description' cannot be found on this object; make sure it exists and is settable.**
Before you start to modify the properties of a content type, you need to load it. Otherwise, you will get an error saying that the property does not exist.

:x:
```
  $ContentType=$Context.Web.ContentTypes.GetByID($ContentTypeGUID)
  $ContentType.Description=$Description
  $ContentType.Update($true)
  $Context.ExecuteQuery()
```
:x:

:heavy_check_mark:
```
  $ContentType=$Context.Web.ContentTypes.GetByID($ContentTypeGUID)
  $Context.Load($ContentType)
  $Context.ExecuteQuery()
  $ContentType.Description=$Description
  $ContentType.Update($true)
  $Context.ExecuteQuery()
```
:heavy_check_mark:

<br/><br/>

### DescriptionResource and NameResource

It seems that DescriptionResource and NameResource are not availble as properties for SharePoint Online content types.
**"'DescriptionResource' is not a member of type 'Microsoft.SharePoint.Client.ContentType'"**

<br/><br/>


### Fields in a sealed content type

Whenever performing any operations on fields of a sealed content type, the operation will fail and the error will be displayed: 
**Exception calling "ExecuteQuery" with "0" argument(s): "Operation is not valid due to the current state of the object."**
Unseal the content type. You may use the following script: 

```
  $ContentType=$Context.Web.ContentTypes.GetByID($ContentTypeGUID)
  $Context.Load($ContentType)
  $Context.ExecuteQuery()
  $ContentType.Sealed=$false
  $ContentType.Update($true)
  $Context.ExecuteQuery()
```

You must have site collection administrator rights to set the Sealed property of an [SPContentType](https://learn.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/ms440819(v=office.14)?redirectedfrom=MSDN) object.  Source: [Updating Content Types on Microsoft Learn](https://learn.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/aa543504(v=office.14)?redirectedfrom=MSDN)

[Script to unseal a content type](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/blob/develop/Content%20Types/Set/Unseal%20a%20content%20type/UnsealCT.ps1)

[Script to unseal all content types in a web](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/blob/develop/Content%20Types/Set/Unseal%20sealed%20content%20types%20in%20site/UnsealAllCT.ps1)
<br/><br/>


### Deleting lookup

When you are trying to delete some of the fields and you receive an error   
**Exception calling "ExecuteQuery" with "0" argument(s): "The primary lookup field cannot be deleted. There are dependent lookups created on this primary lookup field that should be deleted first."**

It suggests that you are trying to delete the source of the lookup column. Delete the lookup first and then try to remove the column again.


<br/><br/>

### Adding columns to a content type
Fields are referred in content types via FieldLinks. The SPFieldCollection object provides developers a way to get a combined view of a column's attributes, as they exist in that content type. Each SPField object represents all the attributes of a column, or a field definition. 

When you access a SPField in a content type, SharePoint merges the field definition with the field reference, and returns the resulting SPField object to you. This prevents developers from having to look up a field definition, and then look up all the attributes in the field definition that are overridden by the field reference for that content type.
Because of this, there is a 1-to-1 correlation between the items in the SPFieldLinkCollection  and SPFieldCollection objects. For each SPFieldLink object that you add to a content type, SharePoint adds a corresponding SPField object that represents the combined view of that column as it is defined in the content type. More on the topic can be found in an [article about Fields and Fieldlinks]( ). 

When you are trying to add a field as XML ```$ContentType.Fields.AddAsXML("XMLSTRING")``` or field as Field ```$ContentType.Fields.Add(Field field)``` to the Field Collection of a content type, you will receive an error:

     Exception calling "ExecuteQuery" with "0" argument(s): "This functionality is unavailable for field collections not associated with a list."
     At C:\Users\ivo\Desktop\wiki\technet\ContentTypes\toPublish\ModifyFieldsvsFieldLinks.ps1:60 char:6
     +      $ctx.ExecuteQuery()
     +      ~~~~~~~~~~~~~~~~~~~
         + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
         + FullyQualifiedErrorId : ServerException
 
Add the column by adding a field to the FieldLinks Collections in order to fix the issue. If you are adding a new column, try creating first a column (e.g. at a site level) and then refer it in your content type as a FieldLink.

```powershell
  $field=$Context.Web.Fields.GetByInternalNameOrTitle($SiteColumnName)
  $link=New-Object Microsoft.SharePoint.Client.FieldLinkCreationInformation
  $link.Field=$field
  $ContentType.FieldLinks.Add($link)
```

<br/><br/>
### Adding fieldlink twice

If you made a mistake and are trying to add the same field link twice (because e.g. the first one is hidden and not visible in the UI), you will receive an error:
**Exception calling "ExecuteQuery" with "0" argument(s): "Unknown Error"**
 

<br/><br/>
### FieldLink

When you decided to add a FieldLink to a SharePoint content type, you first need to create the FieldLink instance and assign its ```.Field``` property. The ```.Field``` property should refer to an existing field. If that property has not been specified you will receive an error

          Exception calling "Add" with "1" argument(s): "The 'parameters.Field' argument cannot be null.
          Parameter name: parameters.Field"
          Specify the .Field parameter before you add the FieldLink to the FieldLink collection.


Solution: 
```powershell
  $field=$Context.Web.Fields.GetByInternalNameOrTitle($SiteColumnName)
  $link=New-Object Microsoft.SharePoint.Client.FieldLinkCreationInformation
  $link.Field=$field
  $ContentType.FieldLinks.Add($link)
```

A sample script:

```powershell
  $Context = New-Object Microsoft.SharePoint.Client.ClientContext($Url)
  $Context.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
  $ContentType = $Context.Web.ContentTypes.GetByID($ContentTypeGUID)
  $Context.Load($ContentType)
  $Context.ExecuteQuery()
  $field=$Context.Web.Fields.GetByInternalNameOrTitle($SiteColumnName)
  $link=New-Object Microsoft.SharePoint.Client.FieldLinkCreationInformation
  $link.Field=$field
  $ContentType.FieldLinks.Add($link)
  $ContentType.Update($false)
  $Context.ExecuteQuery()
```


<br/><br/>
### ReadOnly/ Sealed

Trying to update a sealed or readonly content type shows an error:
**Exception calling "ExecuteQuery" with "0" argument(s): "The content type "Web Part Page" at "/sites/TeamsitewithLibraries/jslink/SitePages" is read only."**

Change the setting before editing the content type.
Unseal the content type. You may use the following script: 

```
  $ContentType=$Context.Web.ContentTypes.GetByID($ContentTypeGUID)
  $Context.Load($ContentType)
  $Context.ExecuteQuery()
  $ContentType.Sealed=$false
  $ContentType.Update($true)
  $Context.ExecuteQuery()
```


<br/><br/>
### Summary




| **Error Message**               | **Possible Reason**             | **How to Fix**              | 
|---------------------------------|---------------------------------|-----------------------------|
| Cannot find an overload for "Update" and the argument count: "0".               | If you do not specify the Boolean value when updating a content type	           | Use $ContentType.Update($true) or $ContentType.Update($false)                  |
| Exception calling "ExecuteQuery" with "0" argument(s): "The content type has no children."           | You are using $ContentType.Update($true) trying to update child content types while the content type has no children           | Use $ContentType.Update($false)               | 
|  Property 'Description' cannot be found on this object; make sure it exists and is settable.            |  The content type has not been loaded                        | Use ```$context.Load($ContentType) $context.ExecuteQuery()``` before trying to update the properties                    | 
| Exception calling "ExecuteQuery" with "0" argument(s): "Operation is not valid due to the current state of the object."  | The content type is sealed.                 | Unseal the content type. Sample script above | 
| Exception calling "ExecuteQuery" with "0" argument(s): "The primary lookup field cannot be deleted. There are dependent lookups created on this primary lookup field that should be deleted first." | You are trying to delete the source of the lookup column     | Remove the lookup first, then try to delete the column                    | 
| Exception calling "ExecuteQuery" with "0" argument(s): "This functionality is unavailable for field collections not associated with a list."               | You are trying to add a column by adding a field to a field collection.                    | Add a field**link**, not a field, to the $ContentType.FieldLinks collection. The fieldlink should be referring to that field.                   | 
|  Exception calling "ExecuteQuery" with "0" argument(s): "Unknown Error"                   |  You are trying to add the same field reference twice                    | Check existing field links                    | 
| Exception calling "Add" with "1" argument(s): "The 'parameters.Field' argument cannot be null. Parameter name: parameters.Field"    | The FieldLink you are trying to add does not have .Field property specified    | Specify the .Field parameter before you add the FieldLink to the FieldLink collection.           |
| Exception calling "ExecuteQuery" with "0" argument(s): "The content type "Web Part Page" at "/sites/TeamsitewithLibraries/jslink/SitePages" is read only."     | The content type is read-only              | Change the .ReadOnly property to $false before updating the content type.               | 
| Exception calling "ExecuteQuery" with "0" argument(s): "The content type "Folder" at "/sites/TeamsitewithLibraries/jslink/SitePages" is sealed."       | The content type is sealed             | Set the content type's property .Sealed to $false.    |

 	 	 


<!-- Default Statcounter code for Content types Known errors
https://powershellscripts.github.io/articles/en/SharePointOnline/SharePoint%20content%20types%20in%20Powershell%20-%20known%20errors/
-->
<script type="text/javascript">
var sc_project=12953200; 
var sc_invisible=0; 
var sc_security="5da4bba5"; 
var sc_client_storage="disabled"; 
var scJsHost = "https://";
document.write("<sc"+"ript type='text/javascript' src='" +
scJsHost+
"statcounter.com/counter/counter.js'></"+"script>");
</script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/12953200/0/5da4bba5/0/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
