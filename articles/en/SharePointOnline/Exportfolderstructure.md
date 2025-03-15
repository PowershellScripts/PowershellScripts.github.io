---
layout: page
title: 'Export your folder structure to XML using Powershell'
menubar: docs_menu
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-01-28'
---



SharePoint has a lot of options allowing you to organize your files. Some of them really amazing. Folders are useful and your users have been familiar with them for a long time before SharePoint, but they aren't necessarily the best way of organizing your files. There is a lot of reasons why not to use them.

But once you have started using SharePoint folders and your users happily created unstructured, repeating, unrecoverable folder mess, you may want to audit them, see the structure of your SharePoint library and find out if you can reorganize, restructure or replace SharePoint folders with managed metadata.


# Retrieve folders

Get folders from SharePoint list or library.

## Step 1: Add paths to SharePoint Online SDK

```powershell
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
```

## Step 2a: Connect to SharePoint Online

Use this step if you are working with SharePoint Online. The rest of the steps are exactly the same for both environments. Create client context and test the connection.

```powershell
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
$ctx.Load($ctx.Web)
$ctx.ExecuteQuery()
```

## Step 2b: Connect to SharePoint Server

Use this step if you are working with SharePoint Server. The rest of the steps are exactly the same for both environments. 

```powershell
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object System.Net.NetworkCredential($Username, $AdminPassword)
$ctx.Load($ctx.Web)
$ctx.ExecuteQuery()
```

## Step 3: Load folders

Load first level folders.

```powershell
$OriginalList=$ctx.Web.Lists.GetByTitle($OriginalLibrary)
 
   # get first level folders from a library
   $folderCollectionFirstLevel=$OriginalList.RootFolder.Folders
   $ctx.load($OriginalList)
   $ctx.Load($folderCollectionFirstLevel)
   $ctx.ExecuteQuery()   
```

The folders may reach several layers down. Some folders may have hundreds of nested folders, some may stop at first level. You can go through such irregular tree structure using recursion:

```powershell
foreach($folder in $folderCollectionFirstLevel)
{ 
    Get-folders -higherLevelFolder $folder -OriginalLibrary $OriginalLibrary      
}
```

where ```Get-folders``` is the name of your function calling itself.



# Create XML


## Step 1: Create XML Document

```powershell
$xmlWriter = New-Object System.XMl.XmlTextWriter($XMLPath,$Null)
$xmlWriter.WriteStartDocument()
$xmlWriter.WriteComment('Get the Information about the folder structure')
$xmlWriter.WriteStartElement('Folders')
$xmlWriter.Flush()
$xmlWriter.Close()
 
$xmlDoc = [System.Xml.XmlDocument](Get-Content $XMLPath);
$xmlDoc.Save($XMLPath)
```


## Step 2: Create xml node 

This step would be easy if we had only one level of folders. It would look like that:

```powershell
$folderNode = $xmlDoc.CreateElement("Folder")
$xmlDoc.SelectSingleNode("/Folders").AppendChild($folderNode)
```

We are dealing, however, with irregular folder structure, sub-folders going maybe hundreds of levels down. We also cannot assume that all folder names will be unique. Partial folder structure may be copied in several places, e.g. folder Invoices in folder Payments may appear in both HR folder and Accounting folder, i.e. Accounting\Payments\Invoices and HR\Payments\Invoices. That means that comparing just the name of a parent folder or even two levels up folder is not a solution. We need to follow the exact path.

## Step 3: Find the parent

The solution starts by counting the number of levels, which corresponds to the number of slashes in ServerRelativeUrl. That allows later to decide at what level the folder is and how deeply we iterate through the nodes:

```powershell
$perspectivenode = "//Folders/"+ $folderServerRelativeUrl.Replace($libraryTitle,"")
$countOfLevels = ($perspectivenode.Length - ($perspectivenode.replace("/","").Length))-3
```
  
ServerRelativeUrl of a sample folder: /test/fold1/fold1-1
$perspectivenode:  //Folders/fold1/fold1-1

  <img src="/articles/images/GitHub-XML1.png" width="400">
  
 
Fig1. ServerRelativeUrl, $perspectivenode variable and created node  

The root in the xml document is called Folders.
```powershell
$tempRelativeUrl=$perspectivenode.Replace("//Folders/","")
$node = $XMLDoc.SelectSingleNode("Folders")
```

Go through all the levels:

```powershell
for($level=0; $level -lt $countOfLevels; $level++)
{
   $nameOfTheUpperFolder = $tempRelativeUrl.Remove($tempRelativeUrl.IndexOf("/"))
   Write-Host "nameoftheupperfolder " $nameOfTheUpperFolder
   $node=$node.SelectNodes("Folder[@Name='$nameOfTheUpperFolder']")
 
   $tempRelativeUrl = $tempRelativeUrl.Remove(0,($tempRelativeUrl.IndexOf("/")+1))
   Write-Host "temprelativerl :" $tempRelativeUrl
}
```


## Step 4: Append child

Each of my items is a folder, so I can hardcode it here.

```powershell
$folderNode = $xmlDoc.CreateElement("Folder")
$node.AppendChild($folderNode)
```

If I were adding different elements, e.g. files and folders, or would like to use different languages ( Ordner in German, etc.), the code would look like this:

```powershell
$newNode = $xmlDoc.CreateElement($nameOfNewNode)
$node.AppendChild($newNode)
```

## Step 5: Add attributes

You can add more or remove these if you don't need them for your scenario.

```powershell
$folderNode.SetAttribute("Name", $folderName)
$folderNode.SetAttribute("NumberOfSubfolders", $noOfSubfolders)
$folderNode.SetAttribute("ItemCount", $folderItemCount)
$folderNode.SetAttribute("ServerRelativeUrl", $folderServerRelativeUrl)
```

## Step 6: Save the document

The last step saves all our changes.

```powershell
$xmlDoc.Save($XMLPath)
```

# Sample results

  <img src="/articles/images/Github-XML2.png" width="400" alt="SharePoint sample results">


# See Also

[Audit folder structure to XML](https://github.com/PowershellScripts/FolderStructure/blob/master/AuditFolderStructureToXML.ps1)

[Audit Sharepoint folder structure to XML (SharePoint Server)](https://github.com/PowershellScripts/FolderStructure/blob/master/AuditFolderStructureToXMLSPServer.ps1)

[Audit SharePoint Online folder structure to XML](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/File%20Management/Audit%20folder%20structure/Audit%20SharePoint%20Online%20folder%20structure%20to%20XML)

[Get the structure of your SharePoint library (folders + files) to XML](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/File%20Management/Audit%20folder%20structure/Get%20the%20structure%20of%20your%20SharePoint%20library%20(folders%20and%20files)%20to%20XML)




<!-- Default Statcounter code for Export folders to XML
https://powershellscripts.github.io/articles/en/SharePointOnline/Exportfolderstructure/
-->
<script type="text/javascript">
var sc_project=12962377; 
var sc_invisible=0; 
var sc_security="217ff976"; 
var sc_client_storage="disabled"; 
var scJsHost = "https://";
document.write("<sc"+"ript type='text/javascript' src='" +
scJsHost+
"statcounter.com/counter/counter.js'></"+"script>");
</script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/12962377/0/217ff976/0/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->

