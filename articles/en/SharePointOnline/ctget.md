Get All Content Types
​Content types can be retrieved as a collection of content types, belonging either to a Web or List.

$ctx.Web.ContentTypes
$ctx.Web.Lists.GetByTitle("Title").ContentTypes

Having loaded the content types collection, one can perform multiple operations, including selecting and modifying a particular content type, updating all of them or deleting several.


Web.ContentTypes vs Web.AvailableContentType

Web.ContentTypes will show you only content types defined at the site level, not all available from the site collection. These content types can be modified and updated.

Web.AvailableContentTypes Jump  retrieves all available content types. These are all content types that apply to the current scope, including those of the current Web site, as well as any parent Web sites. These content types are read only and cannot be modified.

Example

This difference is best visible when you retrieve content types for a subsite, not a site collection. For test purposes, I created a subsite polski in a site collection called TeamsitewithLibraries. To the site collection I added several custom content types which are available at a subsite level. To the subsite I added one content type. It is the only one editable from the polski subsite site settings:

  

This will be the only content type retrieved in the content type collection of

$ctx.Web.ContentTypes

To see the difference I wrote a short script to load and count the content types from both collections .ContentTypes and .AvailableContentTypes

$ctx.Load($ctx.Web.AvailableContentTypes)
 
  foreach( $cc in $ctx.Web.AvailableContentTypes)
  {
             
       $i++
      
  }
     Write-Host "Available content types " $i
         
$ctx.Load($ctx.Web.ContentTypes)

The result is  1 vs 127 available content types.

 

This script is available for download here Jump .


Get All Content Types' Names


Knowing the difference between AvailableContentTypes and ContentTypes collections, feel free to modify any of the scripts below to include one or both collections. For the sake of brevity, only one of the collections (AvailableContentTypes or ContentTypes) is shown below.
 
Let us say that I want to verify what content types are added to my site collection and the best way for me to do it is to see the names of all content types. In order to get the content types added to the site collection, Web.ContentTypes content type collection is used. 

1. Download and install SharePoint Online SDK Jump .

2. Declare all variables. It is a good practice to declare everything dependable on user's input in one place.

  # Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
 
# Insert the credentials and the name of the admin site
$Username="admin@tenant.onmicrosoft.com"
$AdminPassword=Read-Host -Prompt "Password" -AsSecureString
$AdminUrl="https://tenant.sharepoint.com/sites/teamsitewithlibraries/sub Jump "

3. Establish the context.

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)

4. Load the site and the content types collection.

$ctx.Load($ctx.Web)
$ctx.Load($ctx.Web.ContentTypes)
$ctx.ExecuteQuery()

5. For each of the content types write its name to the console. The results can be also exported to a csv file using Out-File cmdlet.

foreach( $cc in $ctx.Web.ContentTypes)
 {
            
         Write-Output $cc.Name
     
    }

The scripts are available for download from Technet Gallery:
Get Names of all Available Content Types Jump
Get Names of All Content Types Jump


Get All Content Types Added to a List


Let us say that we are not interested in the "potential" content types. We want to know the content types that are already in use deployed in the list and what lists are those. It is very useful when you are trying to remove a content type and receive an error message "Another site or list is still using this content type." Let us say we even want a report for that. With Powershell everything* is possible. 

1. Download and install SharePoint Online SDK Jump .

2. Declare all variables. It is a good practice to declare everything dependable on user's input in one place.
  # Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
 
# Insert the credentials and the name of the admin site
$Username="admin@tenant.onmicrosoft.com"
$AdminPassword=Read-Host -Prompt "Password" -AsSecureString
$AdminUrl="https://tenant.sharepoint.com/sites/teamsitewithlibraries/sub Jump "

3. Establish the context.
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)

4. Load the lists. Libraries are a type of list, so they will loaded here as well. The content types are not loaded at the moment. They will be loaded per list basis. 
$ctx.Load($ctx.Web.Lists)
$ctx.ExecuteQuery()

5. Load the content types collection. 
foreach( $ll in $ctx.Web.Lists)
  {
 
        $ctx.Load($ll.ContentTypes)
 
        try
        {
        $ctx.ExecuteQuery()
        }
        catch
        {
        }

6. Write out to console the name of each content type and close the first loop with the lists.
   Write-Host $ll.Title -ForegroundColor Green
        
        foreach($cc in $ll.ContentTypes)
     {
     Write-Output $cc.Name
     }
 
}
Two points here: first of all, there is a loop within a loop, don't forget your brackets! Secondly, did you notice the use of Write-Host and Write-Output functions? Theoretically similar, these functions serve two purposes here. When I want to see the results in the console window, I see the content types neatly divided by the green titles of the lists they belong to:
 

I can also export the results to a .csv file using Out-File cmdlet, and the Write-Host will not be exported. If you want to get a report with content types and their associated lists scroll down to PROPERTIES section. 
Here I am exporting the results to a .csv file, and the appearing green lists' title mark the progress of the script:
 

The results in a CSV file without the lists' titles:
 

The script is available for download from Technet Gallery here Jump . 


Get Selected Content Type

The above scripts concentrated on retrieving all of the available content types. In order to retrieve a single content type, we have to have at least one of its properties, e.g.

ID


Having content type ID allows to retrieve the content type using a function specially designed for that:

GetById($CTID) Jump

where $CTID corresponds to the content type ID.

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
  $ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
  $ctx.ExecuteQuery()
   
  $ctx.Load($ctx.Web)
  $cc=$ctx.Web.ContentTypes.GetById($CTID)
  $ctx.Load($cc)
  $ctx.ExecuteQuery()
 Available from Technet Gallery: Get properties of a single content type by its ID Jump

Array

A less effective method, but entirely sufficient if we want to retrieve only a name for the content type is to leverage the fact that content types is an array of object and each object can be accessed through its place in the array: $ctx.Web.ContentTypes[1].Name
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
 $ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
 $ctx.ExecuteQuery()
  
 $ctx.Load($ctx.Web)
 
 $ctx.Load($ctx.Web.ContentTypes)
 $ctx.ExecuteQuery()
Write-Host $ctx.Web.ContentTypes[1].Name
Available from Technet Gallery: Get Single Content Type - Array Method Jump

Loop

You can loop through the content types collection in order to retrieve a content type corresponding to your requirements. The disadvantage is that the script is not very efficient - nothing noticeable with 100 content types, but if you really went into customizations, and have tens of thousands content types, it may take a while. The advantage is that you can specify any of the properties. The full list of properties is available in the PROPERTIES section of the article below. The scripts mentioned here are only few ways of leveraging those properties - feel free to invent your own! Some of the examples are:
 only content types that are hidden 
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
   
  $ctx.Load($ctx.Web)
  $ctx.Load($ctx.Web.ContentTypes)
  $ctx.ExecuteQuery()
 
     foreach($cc in $ctx.Web.ContentTypes)
     {
        if($cc.Hidden -eq $true)
        {
          Write-Host $cc.Name
        }
     }
Available from Technet Gallery: Get All Hidden Content Types added to the site Jump
content types from a particular group, e.g. Business Intelligence
 


$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
 
$ctx.Load($ctx.Web)
$ctx.Load($ctx.Web.ContentTypes)
$ctx.ExecuteQuery()
 
 
     foreach($cc in $ctx.Web.ContentTypes)
     {
        if($cc.Group -eq "Business Intelligence")
        {
          Write-Host $cc.Name
        }
     }
Available from Technet Gallery: Get content types belonging to a group Jump
 

content types derived from a specific content type, using the original content type's ID
$Username="admin@tenant.onmicrosoft.com"
$AdminPassword=Read-Host -Prompt "Password" -AsSecureString
$AdminUrl="https://tenant.sharepoint.com/sites/powie1 Jump "
$Parent="0x0101009148F5A04DDD49CBA7127AADA5FB792B006973ACD696DC4858A76371B2FB2F439A"

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
$ctx.ExecuteQuery()
$ctx.Load($ctx.Web)
$ctx.Load($ctx.Web.ContentTypes)
$ctx.ExecuteQuery()
 
 
     foreach($cc in $ctx.Web.ContentTypes)
     {
         $ctx.Load($cc.Parent)
         $ctx.ExecuteQuery()
          
 
        if($cc.Parent.ID.ToString().Trim() -eq $Parent)
        {
          Write-Host $cc.Name
        }
     }
Available from Technet Gallery: Get Content Types Derived From One Parent Jump
content types derived from a specific content type, using the original content type's name
$Parent="Audio"
 
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
$ctx.ExecuteQuery()
$ctx.Load($ctx.Web)
$ctx.Load($ctx.Web.ContentTypes)
$ctx.ExecuteQuery()
 
 
     foreach($cc in $ctx.Web.ContentTypes)
     {
         $ctx.Load($cc.Parent)
         $ctx.ExecuteQuery()
          
 
        if($cc.Parent.Name.ToString().Trim() -eq $Parent)
        {
          Write-Host $cc.Name
        }
     }
 
         
        
     
Available from Technet Gallery: Get Content Types Derived From One Parent 2 Jump
check to which content types a given column is added
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
 $ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
 $ctx.ExecuteQuery()
  
 $ctx.Load($ctx.Web)
 $ctx.Load($ctx.Web.ContentTypes)
 $ctx.ExecuteQuery()
 
 
    foreach($cc in $ctx.Web.ContentTypes)
    {
 
$column=$cc.Fields.GetByInternalNameOrTitle($NameOrTitle.Trim())
$ctx.Load($column) 
      $ErrorActionPreference= 'silentlycontinue'
      try
      {
        $ctx.ExecuteQuery()
        }
        catch [Net.WebException]
        {
        }
         
       if($column.Title -eq $NameOrTitle.Trim())
       {
         Write-Host $cc.Name
       }
    }
// $errorActionPreference allows us NOT to see multiple errors for all content types that do not have the column
// the specific column has to be loaded separately 
Available from Technet Gallery: Get Content Types with a particular column Jump
and endless, endless more


Scripts

The above scripts and few more have been packaged into reusable functions and are available for download in Technet Gallery:

Get Single Content Type - Array Method Jump
Get properties of a single content type by its ID Jump
Get All Hidden Content Types added to the site Jump
Get content types belonging to a group Jump
Get Content Types Derived From One Parent Jump
Get Content Types Derived From One Parent 2 Jump
Get content types which cannot be modified Jump
Get Content Types with a particular column Jump


Get Content Type Properties

When creating a content type, ContentTypeCreationInformation is used and offers a limited range of properties. Once a content type has been created, more properties become available.

List of default properties

Description	 
Context Jump	Returns the context that is associated with the client object. (Inherited from ClientObject.) Jump
Description Jump	Gets or sets a description of the content type.
DescriptionResource Jump	 
DisplayFormTemplateName Jump	Gets or sets a value that specifies the name of a custom display form template to use for list items that have been assigned the content type.
DisplayFormUrl Jump	Gets or sets a value that specifies the URL of a custom display form to use for list items that have been assigned the content type.
DocumentTemplate Jump	Gets or sets a value that specifies the file path to the document template used for a new list item that has been assigned the content type.
DocumentTemplateUrl Jump	Gets a value that specifies the URL of the document template assigned to the content type.
EditFormTemplateName Jump	Gets or sets a value that specifies the name of a custom edit form template to use for list items that have been assigned the content type.
EditFormUrl Jump	Gets or sets a value that specifies the URL of a custom edit form to use for list items that have been assigned the content type.
FieldLinks Jump	Gets the column (also known as field) references in the content type.
Fields Jump	Gets a value that specifies the collection of fields for the content type.
Group Jump	Gets or sets a value that specifies the content type group for the content type.
Hidden Jump	Gets or sets a value that specifies whether the content type is unavailable for creation or usage directly from a user interface.
Id Jump	Gets a value that specifies an identifier for the content type. The ContentTypeId type is the identifier for the specified content type. The identifier is a string composed of hexadecimal characters. The identifier must be unique relative to the current site collection and site, and must follow the pattern of prefixing a ContentTypeId with its parent ContentTypeId.
JSLink Jump	Gets or sets the JSLink for the content type custom form template.
MobileDisplayFormUrl Jump	 
MobileEditFormUrl Jump	 
MobileNewFormUrl Jump	 
Name Jump	Gets or sets a value that specifies the name of the content type.
NameResource Jump	 
NewFormTemplateName Jump	Gets or sets a value that specifies the name of a custom new form template to use for list items that have been assigned the content type.
NewFormUrl Jump	Gets or sets a value that specifies the URL of a custom new form to use for list items that have been assigned the content type.
ObjectData Jump	Gets the object data for the current client object. (Inherited from ClientObject.) Jump
ObjectVersion Jump	Gets a string that indicates the version of the current client object. This member is reserved for internal use and is not intended to be used directly from your code. (Inherited from ClientObject.) Jump
Parent Jump	Gets the parent content type of the content type.
Path Jump	Tracks how a client object is created in the ClientRuntimeContext class so that the object can be recreated on the server. This member is reserved for internal use and is not intended to be used directly from your code. (Inherited fromClientObject.)
ReadOnly Jump	Gets or sets a value that specifies whether changes to the content type properties are denied.
SchemaXml Jump	Gets a value that specifies the XML Schema representing the content type.
SchemaXmlWithResourceTokens Jump	Gets a non-localized version of the XML schema that defines the content type.
Scope Jump	Gets a value that specifies a server-relative path to the content type scope of the content type.
Sealed Jump	Gets or sets whether the content type can be modified.
ServerObjectIsNull Jump	Gets the server object and returns null if the server object is null. (Inherited from ClientObject.) Jump
StringId Jump	A string representation of the value of the Id.
Tag Jump	Gets or sets data that is associated with the client object. (Inherited from ClientObject.) Jump
TypedObject Jump	Gets the object with the correct type information returned from the server. (Inherited from ClientObject.) Jump
WorkflowAssociations Jump	Gets a value that specifies the collection of workflow associations for the content type.
Source: https://msdn.microsoft.com/en-us/library/microsoft.sharepoint.client.contenttype_properties.aspx Jump

Collections as properties

Most of the properties can be represented as single string or int value. There are, however, also properties that are more complex. They include multiple values or refer to other objects. Among content type properties, such examples are Fields, FieldLinks, and WorkflowAssociations. Properties such as Fields, FieldLinks and WorkflowAssociations represent whole collections of values and cannot be set directly by mere assignment.

 $contentType.WorkflowAssociations=$MyCoolWorkflow

You can add to the collections or remove their elements, using .Add() Jump  or retrieve the element and delete the object Jump  methods: 

$contenttype.Fields.GetById($FieldId).DeleteObject()

Script

In the example below the script retrieves multiple properties of all content types within a site collection.

1. Open PowerShell ISE (or a Notepad, that is also an option, though not a very convenient one). Enter paths to SharePoint Online SDK Client and Client.Runtime libraries installed on your local computer.

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

2. Create a Microsoft.SharePoint.Client.ClientContext. $Username, $Url and $AdminPassword variables were defined earlier. ExceuteQuery() is not required at this stage. It is a personal preference and allows me to eliminate incorrect credentials as potential errors at an early stage.

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
$ctx.ExecuteQuery()


3. Load the site and the content types.

$ctx.Load($ctx.Web)
$ctx.Load($ctx.Web.ContentTypes)
$ctx.ExecuteQuery()

4. Loop through all content types.

foreach( $cc in $ctx.Web.ContentTypes)
   {
# Code coming here
 
   }

5. Some of the properties that the script retrieves are Fields, FieldLinks and Workflow Associations collections. As whole collections of new data, they have to be requested explicitly. Using property $cc.WorkflowAssociations without requesting earlier the WorkflowAssociations collection at best may return merely a class of Workflow Associations Jump . Otherwise the script may give an error: 
format-default: The collection has not been initialized. It has not been requested or the request has not been executed. It may need to be explicitly requested.
That is why those three collections are loaded separately before creating an instance of a PSObject.

$ctx.Load($cc)
     $ctx.Load($cc.FieldLinks)
     $ctx.Load($cc.Fields)
     $ctx.Load($cc.WorkflowAssociations)
     $ctx.ExecuteQuery()

6. The loop:

foreach( $cc in $ctx.Web.ContentTypes)
 {
                    
    $ctx.Load($cc)
    $ctx.Load($cc.FieldLinks)
    $ctx.Load($cc.Fields)
    $ctx.Load($cc.WorkflowAssociations)
    $ctx.ExecuteQuery()
   
    Write-Output $cc
    }

5. That list of properties can be expanded to include also your own custom properties, added by a script. You can add properties to an object using Add-Member cmdlet Jump . This particular example, creates a new PSObject, and apart from content type properties NAME and DESCRIPTION, adds to the object a custom property WEB, which is equal to the site's url from which the content type is retrieved. This method also allows to define which default properties should be present in the end result. 
 
$obj = New-Object PSObject
$obj | Add-Member NoteProperty Title($cc.Name)
$obj | Add-Member NoteProperty Web($url)
$obj | Add-Member NoteProperty Description($cc.Description)

7. It can also add each of the workflows, fieldlinks and fields as a separate property to the PSObject, using their IDs as PropertyNames and Titles/Names as Values. Bear in mind that what makes a name and what makes a value of a custom property is entirely up to you. Just as well Title/Name could have been the property names (as long as they are unique), and other properties such as $workflow.Created or $fieldlink.Hidden bool could be inserted as values of our custom NoteProperty.
 
foreach($field in $cc.Fields)
     {
      $PropertyName="Field"+$field.ID
      $obj | Add-Member NoteProperty $PropertyName($field.Title)
     }
foreach($fieldlink in $cc.FieldLinks)
     {
      $PropertyName="Fieldlink"+$fieldlink.ID
      $obj | Add-Member NoteProperty $PropertyName($fieldlink.Name)
     }
foreach($workflow in $cc.WorkflowAssociations)
     {
      $PropertyName="Workflow"+$workflow.ID
      $obj | Add-Member NoteProperty $PropertyName($workflow.Name)
     }


8. Add all other properties (Mobile properties are omitted to show that not all available properties must be included).

$obj | Add-Member NoteProperty Group($cc.Group)
    $obj | Add-Member NoteProperty Hidden($cc.Hidden)
    $obj | Add-Member NoteProperty ID($cc.ID)
    $obj | Add-Member NoteProperty JSLink($cc.JSLink)
    $obj | Add-Member NoteProperty NewFormTemplateName($cc.NewFormTemplateName)
    $obj | Add-Member NoteProperty NewFormUrl($cc.NewFormUrl)
    $obj | Add-Member NoteProperty Parent($cc.Parent)
    $obj | Add-Member NoteProperty ReadOnly($cc.ReadOnly)
    $obj | Add-Member NoteProperty SchemaXML($cc.SchemaXML)
    $obj | Add-Member NoteProperty SchemaXmlWithResourceTokens($cc.SchemaXmlWithResourceTokens)
    $obj | Add-Member NoteProperty Scope($cc.Scope)
    $obj | Add-Member NoteProperty Sealed($cc.Sealed)
    $obj | Add-Member NoteProperty ServerObjectIsNull($cc.ServerObjectIsNull)
    $obj | Add-Member NoteProperty StringID($cc.StringID)
    $obj | Add-Member NoteProperty Tag($cc.Tag)
    $obj | Add-Member NoteProperty TypedObject($cc.TypedObject)


9. Full loop:

foreach( $cc in $ctx.Web.ContentTypes)
{ 
          
   $ctx.Load($cc)
   $ctx.Load($cc.FieldLinks)
   $ctx.Load($cc.Fields)
   $ctx.Load($cc.WorkflowAssociations)
   $ctx.ExecuteQuery()
   $obj = New-Object PSObject
   $obj | Add-Member NoteProperty Title($cc.Name)
   $obj | Add-Member NoteProperty  List($ll.Title)
   $obj | Add-Member NoteProperty Web($url)
   $obj | Add-Member NoteProperty Description($cc.Description)
   $obj | Add-Member NoteProperty DisplayFormTemplateName($cc.DisplayFormTemplateName)
   $obj | Add-Member NoteProperty DisplayFormUrl($cc.DisplayFormUrl)
   $obj | Add-Member NoteProperty DocumentTemplate($cc.DocumentTemplate)
   $obj | Add-Member NoteProperty DocumentTemplateUrl($cc.DocumentTemplateUrl)
   $obj | Add-Member NoteProperty EditFormTemplateName($cc.EditFormTemplateName)
   $obj | Add-Member NoteProperty EditFormUrl($cc.EditFormUrl)
 
   foreach($field in $cc.Fields)
   {
    $PropertyName="Field"+$field.ID
    $obj | Add-Member NoteProperty $PropertyName($field.Title)
   }
   foreach($fieldlink in $cc.FieldLinks)
   {
    $PropertyName="Fieldlink"+$fieldlink.ID
    $obj | Add-Member NoteProperty $PropertyName($fieldlink.Name)
   }
 
   $obj | Add-Member NoteProperty Group($cc.Group)
   $obj | Add-Member NoteProperty Hidden($cc.Hidden)
   $obj | Add-Member NoteProperty ID($cc.ID)
   $obj | Add-Member NoteProperty JSLink($cc.JSLink)
   $obj | Add-Member NoteProperty NewFormTemplateName($cc.NewFormTemplateName)
   $obj | Add-Member NoteProperty NewFormUrl($cc.NewFormUrl)
   $obj | Add-Member NoteProperty Parent($cc.Parent)
   $obj | Add-Member NoteProperty ReadOnly($cc.ReadOnly)
   $obj | Add-Member NoteProperty SchemaXML($cc.SchemaXML)
   $obj | Add-Member NoteProperty SchemaXmlWithResourceTokens($cc.SchemaXmlWithResourceTokens)
   $obj | Add-Member NoteProperty Scope($cc.Scope)
   $obj | Add-Member NoteProperty Sealed($cc.Sealed)
   $obj | Add-Member NoteProperty ServerObjectIsNull($cc.ServerObjectIsNull)
   $obj | Add-Member NoteProperty StringID($cc.StringID)
   $obj | Add-Member NoteProperty Tag($cc.Tag)
   $obj | Add-Member NoteProperty TypedObject($cc.TypedObject)
 
   foreach($workflow in $cc.WorkflowAssociations)
   {
    $PropertyName="Workflow"+$workflow.ID
    $obj | Add-Member NoteProperty $PropertyName($workflow.Name)
   }
 
   Write-Output $obj
   }


10. Kinda tedious. Not to mention monotonous copy-pasting, which should suggest that something may not be done the fastest way. Taking $cc variable as an instance of a content type with all the properties already there, and adding to $cc the extra properties is a faster way, but it does not allow us to OMIT any of the default properties.

foreach( $cc in $ctx.Web.ContentTypes)
  {  
     $ctx.Load($cc)
 $ctx.Load($cc.FieldLinks)
     $ctx.Load($cc.Fields)
     $ctx.Load($cc.WorkflowAssociations)
     $ctx.ExecuteQuery()
     $cc | Add-Member NoteProperty Web($url)
      foreach($field in $cc.Fields)
     {
      $PropertyName="Field"+$field.ID
      $cc | Add-Member NoteProperty $PropertyName($field.Title)
     }
     foreach($fieldlink in $cc.FieldLinks)
     {
      $PropertyName="Fieldlink"+$fieldlink.ID
      $cc | Add-Member NoteProperty $PropertyName($fieldlink.Name)
     }
     foreach($workflow in $cc.WorkflowAssociations)
     {
      $PropertyName="Workflow"+$workflow.ID
      $cc | Add-Member NoteProperty $PropertyName($workflow.Name)
     }
     Write-Output $cc
     }                   
         
}


11. There are more possibilities, perhaps even infinite amount of them. There can be more or less properties added, depending on whether we use PSObject or the content type $cc variable. Feel free to customize it up to your needs, and if in doubt - do not hesitate to post a question. I am looking forward to them!  

Full script

The script below is one of the above-mentioned versions, packaged into a re-usable function. For other versions do not hesitate to check TechNet Gallery:
Get all default properties of all content types in one site Jump  
Get All Properties of All Content Types with the custom properties added Jump
Get All Properties of All Content Types in All Lists (Detailed) Jump
Get properties of a single content type by its ID Jump

function Get-SPOContentType
{
   
   param (
   [Parameter(Mandatory=$true,Position=1)]
        [string]$Username,
        [Parameter(Mandatory=$true,Position=2)]
        $AdminPassword,
        [Parameter(Mandatory=$true,Position=3)]
        [string]$Url
        )
   
  $ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
  $ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
  $ctx.ExecuteQuery()
   
  $ctx.Load($ctx.Web)
  $ctx.Load($ctx.Web.ContentTypes)
  $ctx.ExecuteQuery()
  Write-Host
  Write-Host $ctx.Url -BackgroundColor White -ForegroundColor DarkGreen
  foreach( $cc in $ctx.Web.ContentTypes)
  {
             
            
     $ctx.Load($cc)
          $ctx.Load($cc.FieldLinks)
     $ctx.Load($cc.Fields)
     $ctx.Load($cc.WorkflowAssociations)
     $ctx.ExecuteQuery()
     $cc | Add-Member NoteProperty Web($url)
      foreach($field in $cc.Fields)
     {
      $PropertyName="Field"+$field.ID
      $cc | Add-Member NoteProperty $PropertyName($field.Title)
     }
     foreach($fieldlink in $cc.FieldLinks)
     {
      $PropertyName="Fieldlink"+$fieldlink.ID
      $cc | Add-Member NoteProperty $PropertyName($fieldlink.Name)
     }
     foreach($workflow in $cc.WorkflowAssociations)
     {
      $PropertyName="Workflow"+$workflow.ID
      $cc | Add-Member NoteProperty $PropertyName($workflow.Name)
     }
     Write-Output $cc
     }    
     }
  
 # Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
 
# Insert the credentials and the name of the admin site
$Username="admin@tenant.onmicrosoft.com"
$AdminPassword=Read-Host -Prompt "Password" -AsSecureString
$AdminUrl="https://tenant.sharepoint.com/sites/teamsitewithlibraries Jump " 
 
Get-SPOContentType -Username $Username -AdminPassword $AdminPassword -Url $AdminUrl


Downloads
