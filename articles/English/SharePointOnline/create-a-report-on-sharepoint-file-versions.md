Create a report on SharePoint file versions
Table of Contents
Overview
Supported lists and libraries
When versions are created
Retrieving versions for all files in one library
Script
Summary
See Also
Versioning and SharePoint: the PowerShell perspective series
GitHub scripts


Overview

Supported lists and libraries
Versioning is available for list items in all default list types—including calendars, issue tracking lists, and custom lists. It is also available for all file types that can be stored in libraries, including Web Part pages. By default, versioning is turned off (apart from new OneDrive for Business libraries  Jump )

When versions are created
There are several actions that trigger version creation. When versioning is enabled, versions are created in the following situations:

* When a list item or file is first created or when a file is uploaded. Note that if you have enabled a library setting that requires file checkout, you have to check the file in to create its first version.

* When a file is uploaded that has the same name as an existing file and the Add as a new version to existing files check box is selected.

* When the properties of a list item or file are changed.

* When a file is opened, edited, and saved. A version is created when you first click Save. It retains the same version number for the duration of the current editing session. Even though you might save it several times, the version remains the same. When you close it and then reopen it for another editing session, another version is created.

* During co-authoring of a document, when a different user begins working on the document or when a user clicks save to upload changes to the library. The default time period for creating new versions during co-authoring is 30 minutes. This is configurable per web application in SharePoint on-premises. This setting is not configurable in SharePoint Online.

Based on: How does versioning work in a list or library? Jump

Retrieving versions for all files in one library

Script

1. Add the necessary libraries
#Paths to SDK
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll" 
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll" 

2. Create the context and test initial input settings (credentials, urls, library paths) by calling .ExecuteQuery()
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $password)
$ctx.Load($ctx.Web)
$ctx.ExecuteQuery()

3. Load the list or library in question
$ll=$ctx.Web.Lists.GetByTitle($ListTitle)
$ctx.Load($ll)
$ctx.ExecuteQuery()

4. Configure the CAMLQuery. It will determine whether what items exactly will be pulled. Scope set to RecursiveAll means that the query should loop through all folders and folder levels for all ITEMS.  
$spqQuery = New-Object Microsoft.SharePoint.Client.CamlQuery
$spqQuery.ViewXml ="<View Scope='RecursiveAll' />"
$itemki=$ll.GetItems($spqQuery)
$ctx.Load($itemki)
$ctx.ExecuteQuery()

5. Items are not the same as files, but each file is also a list item and they share several properties, such as ServerRelativeUrl. The common properties allow us to identify the files and load their respective versions 
foreach($item in $itemki)
 {
 
 Write-Host $item["FileRef"]
 $file =
       $ctx.Web.GetFileByServerRelativeUrl($item["FileRef"]);
       $ctx.Load($file)
       $ctx.Load($file.Versions)
       $ctx.ExecuteQuery()

6. I decided to create 2 separate reports. One with file versions, and one with files that did not have any versions. It is up to you to adjust it to your requirements.
    if ($file.Versions.Count -eq 0)
      {
        $obj=New-Object PSObject
        $obj | Add-Member NoteProperty ServerRelativeUrl($file.ServerRelativeUrl)
        $obj | Add-Member NoteProperty FileLeafRef($item["FileLeafRef"])
        $obj | Add-Member NoteProperty Versions("No Versions Available")
 
        $obj | export-csv -Path $CSVPath2 -Append
      }
 
 
      Foreach ($versi in $file.Versions){
 
      $user=$versi.CreatedBy
      $ctx.Load($versi)
      $ctx.Load($user)
      $ctx.ExecuteQuery()
      $versi | Add-Member NoteProperty CreatedByUser($user.LoginName)
      $versi | Add-Member NoteProperty FileLeafRef($item["FileLeafRef"])
       $versi |export-csv -Path $CSVPath -Append
                  }
}

Summary

The script by design throws non-terminating errors during execution if it encounters folders or document sets which do not possess versions. It is possible to eliminate them, e.g. by excluding content types such as Folder:
if $ContentTypeID -eq ""
or by requiring only Files:
if($file.TypedObject.ToString() -eq "Microsoft.SharePoint.Client.File")

It is, however, not necessary, as the errors do not influence the outcome in the report and, in a very personal opinion, are signs of healthy script execution.* 
