---
layout: page
title: 'Report on files with unique permissions'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-02-09'
---


Show your users which files have unique permissions. 

Files with unique permissions often have different individuals granted access. It is crucial to identify them for security and administrative purposes. It is important that site and library owners review the files and the granted access regularly.

Using PowerShell, you can generate a detailed report of such files and export the list to a CSV file for easy reference. Additionally, consider creating a custom view in SharePoint Online. This way users can directly see files which require their attention.



# Generate the report


```Powershell

# Define the list name
$listName = "YourListName"

# Path for the output CSV file
$outputCsvPath = "C:\Path\To\Output\UniquePermissionsReport.csv"

# Get all list items
$items = Get-PnPListItem -List $listName -PageSize 5000 -Fields "FileRef", "FileLeafRef", "Modified"

# Initialize an array to store details of items with unique permissions
$uniqueItems = @()

# Iterate through each item to check for unique permissions
foreach ($item in $items) {
    # Check if the item has unique permissions
    if ($item.HasUniqueRoleAssignments) {
        # Add item details to the array
        $uniqueItems += [PSCustomObject]@{
            Name            = $item["FileLeafRef"]
            FileRef         = $item["FileRef"]
            LastModified    = $item["Modified"]
        }
    }
}

# Export the details to a CSV file
$uniqueItems | Export-Csv -Path $outputCsvPath -NoTypeInformation -Encoding UTF8

# Output the result
Write-Host "Details of items with unique permissions have been exported to $outputCsvPath"


```