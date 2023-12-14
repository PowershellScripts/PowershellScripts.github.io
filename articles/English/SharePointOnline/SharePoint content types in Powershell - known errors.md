Introduction

This article is a helper article for the main compendium on Content Types:
SharePoint Online content types in Powershell: Add
SharePoint Online content types in Powershell: Get
SharePoint Online content types in Powershell: Edit


Known Errors

The errors below are based on THOUSANDS of tests performed on various Office 365 tenants. That by no means makes them definite, final or in any way exclusive. Bear in mind that the same error messages may have several causes and treat the descriptions below as suggestions for troubleshooting, not a definite and only possible cause. 


Updating the content types

When you update the content type remember to specify whether the child content types should be updated as well.
If you do not specify the Boolean value, an error will occur Cannot find an overload for "Update" and the argument count: "0".
If you set to $true, but you are trying to update a content type without child content types, e.g. a list content type  Exception calling "ExecuteQuery" with "0" argument(s): "The content type has no children."


Load the Content Type

Before you start to modify the properties of a content type, you need to load it. Otherwise, you will get an error saying that the property does not exist.
Property 'Description' cannot be found on this object; make sure it exists and is settable.


DescriptionResource and NameResource

It seems that DescriptionResource and NameResource are not availble as properties for SharePoint Online content types.
 "'DescriptionResource' is not a member of type 'Microsoft.SharePoint.Client.ContentType'" 


DisplayFormTemplateName

The statements provided here where the source is not quoted come from tests of the default behaviour. The tests were conducted on three different tenants  two weeks apart and behaviour proved consistent which led to the conclusion that it is by design. However, in case of the DisplayFormTemplateName, on occasion, setting the incorrect name or different one did not bring the change in the behaviour visible in UI. So even though the DisplayFormTemplateName of an item was set to "MadeUp", the item would open correctly. As said the above-mentioned behaviour occurred only on occasion and was in clear minority. So in the chapter on DisplayFormTemplateName the dominating behaviour was described.


Fields in a sealed content type

Whenever performing any operations on fields of a sealed content type, the operation will fail and the error will be displayed: 
Exception calling "ExecuteQuery" with "0" argument(s): "Operation is not valid due to the current state of the object."
Unseal the content type. You may use the following script: "Unseal" sealed content types in SharePoint Online site collection Jump
You must have site collection administrator rights to set the Sealed property of an SPContentType Jump  object.[1] Jump

Deleting lookup

When you are trying to delete some of the fields and you receive an error   
Exception calling "ExecuteQuery" with "0" argument(s): "The primary lookup field cannot be deleted. There are dependent lookups created on this primary lookup field that should be deleted first."
it suggests that you are trying to delete the source of the lookup column. Delete the lookup first and then try to remove the column again.


Adding columns to a content type
Fields are referred in content types via FieldLinks. The SPFieldCollection Jump object serves only to provide developers a way to get a combined view of a column's attributes, as they exist in that content type. Each SPField Jump object represents all the attributes of a column, or field, definition, merged with those attributes that have been overridden in the field reference for that content type. 

When you access a SPField  Jump  in a content type, SharePoint Foundation merges the field definition with the field reference, and returns the resulting SPField  Jump  object to you. This prevents developers from having to look up a field definition, and then look up all the attributes in the field definition that are overridden by the field reference for that content type.
Because of this, there is a 1-to-1 correlation between the items in the SPFieldLinkCollection Jump  and SPFieldCollection Jump objects. For each SPFieldLink Jump object that you add to a content type, SharePoint Foundation adds a corresponding SPField Jump object that represents the combined view of that column as it is defined in the content type. More on the topic can be found here. 

When you are trying to add a field as XML ContentType.Fields.AddAsXML("XMLSTRING") or field as Field ContentType.Fields.Add(Field field) to the Field Collection of a content type, you will receive an error:

Exception calling "ExecuteQuery" with "0" argument(s): "This functionality is unavailable for field collections not associated with a list."
At C:\Users\ivo\Desktop\wiki\technet\ContentTypes\toPublish\ModifyFieldsvsFieldLinks.ps1:60 char:6
+      $ctx.ExecuteQuery()
+      ~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : ServerException
 
Add the column by adding a field to the FieldLinks Collections in order to fix the issue. If you are adding a new column, try creating first a column (e.g. at a site level) and then refer it in your content type as a FieldLink, e.g. in Powershell or Console Application as in the example here Jump .


Twice

If you made a mistake and are trying to add the same field link twice (because e.g. the first one is hidden and not visible in the UI), you will receive an error:
Exception calling "ExecuteQuery" with "0" argument(s): "Unknown Error"
when you are trying to add the same field reference twice 


FieldLink

When you decided to add a FieldLink, you first need to create the FieldLink instance and assign its .Field property. The Field property should refer to an existing field. If that property has not been specified you will receive an error
Exception calling "Add" with "1" argument(s): "The 'parameters.Field' argument cannot be null.
Parameter name: parameters.Field"
Specify the .Field parameter before you add the FieldLink to the FieldLink collection.


ReadOnly/ Sealed


Trying to update a sealed or readonly content type shows an error:
Exception calling "ExecuteQuery" with "0" argument(s): "The content type "Web Part Page" at "/sites/TeamsitewithLibraries/jslink/SitePages" is read only."

 

Change the setting before editing the content type.


Summary


Error Message 	Possible Reason 	How to Fix 
 Cannot find an overload for "Update" and the argument count: "0".


 If you do not specify the Boolean value when updating a content type	 Use $ContentType.Update($true) or $ContentType.Update($false) 
 Exception calling "ExecuteQuery" with "0" argument(s): "The content type has no children."


 You are using $ContentType.Update($true) trying to update child content types while the content type has no children	 Use $ContentType.Update($false) 
 Property 'Description' cannot be found on this object; make sure it exists and is settable.


 The content type has not been loaded	 Use $context.Load($ContentType)
$context.ExecuteQuery()
before trying to update the properties
'DescriptionResource' is not a member of type 'Microsoft.SharePoint.Client.ContentType'


 May apply to DescriptionResource or NameResource.
The property may not be available for SharePoint Online.	 -
 Exception calling "ExecuteQuery" with "0" argument(s): "Operation is not valid due to the current state of the object."


 The content type is sealed	 Set the content type's property .Sealed to $false. You may use the following solution: "Unseal" sealed content types in SharePoint Online site collection Jump  Jump
You must have site collection administrator rights to set the Sealed property of an SPContentType Jump  object.[1] Jump
 Exception calling "ExecuteQuery" with "0" argument(s): "The primary lookup field cannot be deleted. There are dependent lookups created on this primary lookup field that should be deleted first."


 You are trying to delete the source of the lookup column	 Remove the lookup first, then try to delete the column
 Exception calling "ExecuteQuery" with "0" argument(s): "This functionality is unavailable for field collections not associated with a list."


 You are trying to add a column by adding a field to a field collection.	 Add a fieldlink referring to that field to the $ContentType.FieldLinks collection
 Exception calling "ExecuteQuery" with "0" argument(s): "Unknown Error"


 You are trying to add the same field reference twice 	 Check existing field links
 Exception calling "Add" with "1" argument(s): "The 'parameters.Field' argument cannot be null.
Parameter name: parameters.Field"


The FieldLink you are trying to add does not have .Field property specified 	 Specify the .Field parameter before you add the FieldLink to the FieldLink collection.
 Exception calling "ExecuteQuery" with "0" argument(s): "The content type "Web Part Page" at "/sites/TeamsitewithLibraries/jslink/SitePages" is read only."


 The Content type is read-only	 Change the .ReadOnly property to $false before updating the content type.
 Exception calling "ExecuteQuery" with "0" argument(s): "The content type "Folder" at "/sites/TeamsitewithLibraries/jslink/SitePages" is sealed."	 The Content type is sealed	Set the content type's property .Sealed to $false. You may use the following solution: "Unseal" sealed content types in SharePoint Online site collection Jump
You must have site collection administrator rights to set the Sealed property of an SPContentType Jump  object.[1] Jump
 	 	 
 	 	 

Related Articles
SharePoint Online content types in Powershell: Add
SharePoint Online content types in Powershell: Get
SharePoint Online content types in Powershell: Edit
