---
layout: page
title: 'Count all files recursively inside a specific folder using Powershell'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-08-10'
---

# Introduction
Using PnP Powershell you can programmatically count all files inside your list or a specific folder.


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

