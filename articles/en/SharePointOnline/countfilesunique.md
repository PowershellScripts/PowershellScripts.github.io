---
layout: page
title: 'Count files or items with unique permissions using Powershell'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-08-11'
---

# Introduction

Using PnP Powershell you can programmatically count files in a folder or the entire library in SharePoint Online that have unique permissions.


# Count files with unique permissions in a folder

The following PnP cmdlets count files with unique permissions in a folder.


```powershell
# Define the folder URL relative to the document library
$folderUrl = "/sites/yoursite/Shared Documents/YourFolder"
$list = "Shared Documents"

# Initialize the count for items with unique permissions
$fileCountWithUniquePermissions = 0;

# Get the folder
$folder = Get-PnPFolder -Url $folderUrl -Includes Files, Folders

foreach ($file in $folder.Files) {
    $fileItem = Get-PnPListItem -List $list -Id $file.ListItemAllFields.Id
    if ($fileItem.HasUniqueRoleAssignments) {
        $fileCountWithUniquePermissions++
    }
}


# Output the total count
Write-Host "Total files:  $fileCountWithUniquePermissions"

```



# Count files or items with unique permissions in a list

The following lines can help you count files in a document library with unique permissions or items with unique permissions in a list. 

```powershell
# Define the list name
$listName = "YourListName"  

# Get all list items
$items = Get-PnPListItem -List $listName -PageSize 5000

# Initialize the count for items with unique permissions
$uniquePermissionCount = 0

# Iterate through each item to check for unique permissions
foreach ($item in $items) {
    # Check if the item has unique permissions
    if ($item.HasUniqueRoleAssignments) {
        # Increment the count if unique permissions are found
        $uniquePermissionCount++
    }
}

# Output the total count of items with unique permissions
Write-Host "Total items with unique permissions: $uniquePermissionCount"

```


# See Also

[Count Files on GitHub](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/File%20Management/CountFiles) 




<!-- Default Statcounter code for Count files unique
https://powershellscripts.github.io/articles/en/SharePointOnline/countfilesunique/
-->
<script type="text/javascript">
var sc_project=13027283; 
var sc_invisible=1; 
var sc_security="ba505107"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13027283/0/ba505107/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->