---
layout: page
title: 'Export your folder structure to XML using Powershell'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2023-12-10'
---
<title> Export your folder structure to XML using Powershell </title>


SharePoint has a lot of options allowing you to organize your files. Some of them really amazing. Folders are useful and your users have been familiar with them for a long time before SharePoint, but they aren't necessarily the best way of organizing your files. There is a lot of reasons why not to use them.

But once you have used them and your users happily created unstructured, repeating, unrecoverable mess, you may want to audit them, see the structure of your library and find out if you can reorganize, restructure or replace them with metadata.


Retrieve folders

Get folders from SharePoint list or library.

Step 1: Add paths to SharePoint Online SDK


Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

Step 2a: Connect to SharePoint Online

Create client context and test the connection.
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
$ctx.Load($ctx.Web)
$ctx.ExecuteQuery()

Step 2b: Connect to SharePoint Server

This solution can work with either SharePoint Online or SharePoint Server. Choose step A or step B depending on your environment. 
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object System.Net.NetworkCredential($Username, $AdminPassword)
$ctx.Load($ctx.Web)
$ctx.ExecuteQuery()


Step 3: Load folders

Load first level folders.
$OriginalList=$ctx.Web.Lists.GetByTitle($OriginalLibrary)
 
   # get first level folders from a library
   $folderCollectionFirstLevel=$OriginalList.RootFolder.Folders
   $ctx.load($OriginalList)
   $ctx.Load($folderCollectionFirstLevel)
   $ctx.ExecuteQuery()   

The folders may reach several layers down. Some folders may have hundreds of nested folders, some may stop at first level. You can go through such irregular tree structure using recursion:
foreach($folder in $folderCollectionFirstLevel)
{ 
    Get-folders -higherLevelFolder $folder -OriginalLibrary $OriginalLibrary      
}
where Get-folders is the name of your function calling itself



Create XML


Step 1: Create XML Document


$xmlWriter = New-Object System.XMl.XmlTextWriter($XMLPath,$Null)
$xmlWriter.WriteStartDocument()
$xmlWriter.WriteComment('Get the Information about the folder structure')
$xmlWriter.WriteStartElement('Folders')
$xmlWriter.Flush()
$xmlWriter.Close()
 
$xmlDoc = [System.Xml.XmlDocument](Get-Content $XMLPath);
$xmlDoc.Save($XMLPath)



Step 2: Create xml node 

This step would be easy if we had only one level of folders. It would look like that:
$folderNode = $xmlDoc.CreateElement("Folder")
$xmlDoc.SelectSingleNode("/Folders").AppendChild($folderNode)

We are dealing, however, with irregular structure, going maybe hundreds of levels down. We also cannot assume that all folder names will be unique. Partial folder structure may be copied in several places, e.g. folder Invoices in folder Payments may appear in both HR folder and Accounting folder, i.e. Accounting\Payments\Invoices and HR\Payments\Invoices. That means that comparing just the name of a parent folder or even two levels up folder is not a solution. We need to follow the exact path.

Step 3: Find the parent

The solution starts by counting the number of levels, which corresponds to the number of slashes in ServerRelativeUrl. That allows later to decide at what level the folder is and how deeply we iterate through the nodes:
$perspectivenode = "//Folders/"+ $folderServerRelativeUrl.Replace($libraryTitle,"")
$countOfLevels = ($perspectivenode.Length - ($perspectivenode.replace("/","").Length))-3
  
ServerRelativeUrl of a sample folder: /test/fold1/fold1-1
$perspectivenode:  //Folders/fold1/fold1-1


 
Fig1. ServerRelativeUrl, $perspectivenode variable and created node  

The root in the xml document is called Folders.  
$tempRelativeUrl=$perspectivenode.Replace("//Folders/","")
$node = $XMLDoc.SelectSingleNode("Folders")

Go through all the levels:
for($level=0; $level -lt $countOfLevels; $level++)
{
   $nameOfTheUpperFolder = $tempRelativeUrl.Remove($tempRelativeUrl.IndexOf("/"))
   Write-Host "nameoftheupperfolder " $nameOfTheUpperFolder
   $node=$node.SelectNodes("Folder[@Name='$nameOfTheUpperFolder']")
 
   $tempRelativeUrl = $tempRelativeUrl.Remove(0,($tempRelativeUrl.IndexOf("/")+1))
   Write-Host "temprelativerl :" $tempRelativeUrl
}



Step 4: Append child

Each of my items is a folder, so I can hardcode it here.
$folderNode = $xmlDoc.CreateElement("Folder")
$node.AppendChild($folderNode)

If I were adding different elements, e.g. files and folders, or would like to use different languages ( Ordner in German, etc.), the code would look like this:
$newNode = $xmlDoc.CreateElement($nameOfNewNode)
$node.AppendChild($newNode)


Step 5: Add attributes

You can add more or remove these if you don't need them for your scenario.
$folderNode.SetAttribute("Name", $folderName)
$folderNode.SetAttribute("NumberOfSubfolders", $noOfSubfolders)
$folderNode.SetAttribute("ItemCount", $folderItemCount)
$folderNode.SetAttribute("ServerRelativeUrl", $folderServerRelativeUrl)


Step 6: Save the document

The last step saves all our changes. 
$xmlDoc.Save($XMLPath)


Sample results

 

Downloads

Audit folder structure to XML
Audit Sharepoint folder structure to XML (SharePoint Server)
Audit SharePoint Online folder structure to XML
Get the structure of your SharePoint library (folders + files) to XML
