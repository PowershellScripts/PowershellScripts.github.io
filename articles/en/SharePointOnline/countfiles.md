---
layout: page
title: 'Count files in a folder using Powershell'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-08-10'
---

# Introduction
Using PnP Powershell you can programmatically count files in a folder or the entire library.

<br/><br/>

# Count files inside a specific folder
The following PnP cmdlets can count files in a folder.

<br/>

### Option 1
Count the files collection in a folder.

```powershell
# Define the folder URL relative to the document library
$folderUrl = "/sites/yoursite/Shared Documents/YourFolder"

# Get the folder
$folder = Get-PnPFolder -Url $folderUrl -Includes Files, Folders

# Initialize file count
$fileCount = $folder.Files.Count

# Output the total count
Write-Host "Total files: $fileCount"

```

<br/>

### Option 2
Count the items from the folder that have the type File.

```powershell

# Define the specific folder path
$folderRelativeUrl = "/sites/yoursite/Shared Documents/YourFolder"

# Retrieve the folder
$folder = Get-PnPFolder -Url $folderRelativeUrl

# Retrieve all files in the folder
$files = Get-PnPFolderItem -Folder $folder -ItemType File

# Count total number of files in the folder
$totalFileCount = $files.Count


# Output the results
Write-Output "Total number of files in the folder: $totalFileCount"
```

<br/><br/>

# Count files in a folder **recursively**
Recursively means exploring all directories and subdirectories within a given directory, processing each level until all contents have been handled.

If you have a folder structure like this:


RootFolder
│
├── SubFolder1
│   ├── File1.txt
│   └── SubSubFolder1
│       └── File2.txt
│
└── SubFolder2
    └── File3.txt


A recursive function to list all files would:

1. Start in RootFolder.
2. List the files directly in RootFolder (if any).
3. Identify subfolders (SubFolder1, SubFolder2).
4. Move into SubFolder1, list its files, and identify any further subfolders (like SubSubFolder1).
5. Continue this process until all subfolders and their files have been listed.
6. The recursive function ensures that no files are missed, regardless of how deeply nested they are within the folder structure.

<br/>

### Option 1
Count files in a folder by building your own recursion.

```powershell
# Define the folder URL relative to the document library
$folderUrl = "/sites/yoursite/Shared Documents/YourFolder"

# Get the folder
$folder = Get-PnPFolder -Url $folderUrl -Includes Files, Folders

# Initialize file count
$fileCount = 0

# Function to count files recursively
function Count-Files($folder) {
    param ($folder)
    
    # Count files in the current folder
    $fileCount += $folder.Files.Count
    
    # Recurse into each subfolder
    foreach ($subFolder in $folder.Folders) {
        Count-Files -folder $subFolder
    }
}

# Start counting files from the root folder
Count-Files -folder $folder

# Output the total count
Write-Host "Total files: $fileCount"

```

<br/>

### Option 2
Count items in a folder that have the type File, using PnP cmdlet `Get-PnPFolderItem -Folder $folder -ItemType File -Recursive` cmdlet. It returns all documents in a folder.

```powershell

# Define the specific folder path
$folderRelativeUrl = "/sites/yoursite/Shared Documents/YourFolder"

# Retrieve the folder
$folder = Get-PnPFolder -Url $folderRelativeUrl

# Retrieve all files in the folder
$files = Get-PnPFolderItem -Folder $folder -ItemType File -Recursive

# Count total number of files in the folder
$totalFileCount = $files.Count


# Output the results
Write-Output "Total number of files in the folder: $totalFileCount"
```


<br/><br/>

# Count files inside a Library

Using `Get-PnPFolderItem` you can also count the files inside the entire SharePoint library.


```powershell

# Define the document library name
$libraryName = "Shared Documents" 

# Get the files in the root of the document library
$files = Get-PnPFolderItem -Folder $libraryName -ItemType File

# Count the files
$fileCount = $files.Count

# Output the count
Write-Output "Number of files in the root of '$libraryName': $fileCount"
```


<br/><br/>

# Count files inside a Library **recursively**

The following 2 options show how to count files in a library recursively.

<br/>

### Option 1

```powershell

# Define the document library name
$libraryName = "Shared Documents"  

# Function to count files recursively
function Get-FileCountRecursively {
    param (
        [string]$folderPath
    )

    # Initialize file count
    $totalFileCount = 0

    # Get all files in the current folder
    $files = Get-PnPFolderItem -Folder $folderPath -ItemType File
    $totalFileCount += $files.Count

    # Get all subfolders
    $subfolders = Get-PnPFolderItem -Folder $folderPath -ItemType Folder

    foreach ($subfolder in $subfolders) {
        $subfolderPath = "$folderPath/$($subfolder.Name)"
        # Recursively count files in each subfolder
        $totalFileCount += Get-FileCountRecursively -folderPath $subfolderPath
    }

    return $totalFileCount
}

# Start the count from the root of the document library
$totalFiles = Get-FileCountRecursively -folderPath $libraryName

# Output the total file count
Write-Output "Total number of files in '$libraryName' (including subfolders): $totalFiles"
```

<br/>

### Option 2

```powershell

# Define the document library name
$libraryName = "Documents"  

# Get the root folder of the document library
$rootFolder = Get-PnPList -Identity $libraryName | Get-PnPProperty -Property RootFolder

# Initialize file count
$fileCount = 0

# Function to count files recursively in a folder
function Count-Files($folder) {
    param ($folder)
    
    # Count files in the current folder
    $fileCount += ($folder | Get-PnPFolderItem -ItemType File).Count
    
    # Get all subfolders and recurse into each one
    $subFolders = Get-PnPFolderItem -FolderSiteRelativeUrl $folder.ServerRelativeUrl -ItemType Folder
    foreach ($subFolder in $subFolders) {
        Count-Files -folder $subFolder
    }
}

# Start counting files from the root folder of the document library
Count-Files -folder $rootFolder

# Output the total count
Write-Host "Total files in the document library '$libraryName': $fileCount"

```


<br/><br/>

# Count all items - files and folders - inside a Library **recursively**

If you don't need to differentiate between files and folders, `Get-PnPListItem` gives you the quickest overview of the SharePoint library size. You can also use this to count items in a list.

```powershell

# Define the document library name
$libraryName = "Shared Documents"  

# Get all items in the document library
$items = Get-PnPListItem -List $libraryName -PageSize 5000 -Fields "FileLeafRef"

# Count the number of items (both files and folders)
$itemCount = $items.Count

# Output the total item count
Write-Output "Total number of items in '$libraryName': $itemCount"
```


<br/><br/>

# Count files in a subfolder

If you want to count files in all immediate subfolders of a SharePoint document library, you can use `Get-PnPFolderItem`. The script here gets a count of files in each immediate subfolder of the library and then exports those numbers into a CSV file.

```powershell

# Define the document library name
$libraryName = "Shared Documents"  

# Define the path for the CSV output
$outputCsvPath = "C:\path\to\output\file.csv" 

# Function to count files in a given folder
function Get-FileCountInFolder {
    param (
        [string]$folderUrl
    )

    # Get all items in the folder
    $items = Get-PnPFolderItem -Folder $folderUrl -ItemType File

    # Return the count of files
    return $items.Count
}

# Get all items (files and folders) in the root of the document library
$rootItems = Get-PnPFolderItem -Folder $libraryName

# Filter out only folders
$subfolders = $rootItems | Where-Object { $_.ItemType -eq "Folder" }

# Prepare an array to hold the results
$results = @()

# Iterate through each subfolder to count files
foreach ($subfolder in $subfolders) {
    $subfolderPath = $subfolder.ServerRelativeUrl
    
    # Count files in the current subfolder
    $fileCount = Get-FileCountInFolder -folderUrl $subfolderPath

    # Create an object with the results
    $result = [PSCustomObject]@{
        FolderPath = $subfolderPath
        FileCount  = $fileCount
    }

    # Add the result to the results array
    $results += $result
}

# Export results to CSV
$results | Export-Csv -Path $outputCsvPath -NoTypeInformation
```



<br/><br/>

# Count files in a subfolder **recursively**

If you want to count files in all immediate subfolders of a SharePoint document library, you can use `Get-PnPFolderItem`. The script here gets a count of files in each immediate subfolder of the library and their subfolders. The results are then exported into a CSV file.

```powershell

# Define the document library name
$libraryName = "Shared Documents"  

# Define the path for the CSV output
$outputCsvPath = "C:\path\to\output\file.csv"  

# Function to count files in a given folder
function Get-FileCountInFolder {
    param (
        [string]$folderUrl
    )

    # Get all items in the folder
    $items = Get-PnPFolderItem -Folder $folderUrl -ItemType File -Recursive

    # Return the count of files
    return $items.Count
}

# Get all items (files and folders) in the root of the document library
$rootItems = Get-PnPFolderItem -Folder $libraryName

# Filter out only folders
$subfolders = $rootItems | Where-Object { $_.ItemType -eq "Folder" }

# Prepare an array to hold the results
$results = @()

# Iterate through each subfolder to count files
foreach ($subfolder in $subfolders) {
    $subfolderPath = $subfolder.ServerRelativeUrl
    
    # Count files in the current subfolder
    $fileCount = Get-FileCountInFolder -folderUrl $subfolderPath

    # Create an object with the results
    $result = [PSCustomObject]@{
        FolderPath = $subfolderPath
        FileCount  = $fileCount
    }

    # Add the result to the results array
    $results += $result
}

# Export results to CSV
$results | Export-Csv -Path $outputCsvPath -NoTypeInformation
```

<br/><br/>

# See Also

[Count Files on GitHub](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/File%20Management/CountFiles) 

[Count Files with Unique Permissions](https://powershellscripts.github.io/articles/en/SharePointOnline/countfilesunique/)


<!-- Default Statcounter code for count files
https://powershellscripts.github.io/articles/en/SharePointOnline/countfiles/
-->
<script type="text/javascript">
var sc_project=13027282; 
var sc_invisible=1; 
var sc_security="725c75d3"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13027282/0/725c75d3/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->