---
layout: page
title: 'Report on files with unique permissions'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-02-15'
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

<br/><br/>

# Create a view

With Powershell you can also shows your users all files with unique permissions. The following script creates a view in your library that displays files with different permissions than the entire library.


```Powershell

# Define the list name and site URL
$listName = "YourListName"
$siteUrl = "https://yourtenant.sharepoint.com/sites/yoursite"

# Path for the output CSV file
$outputCsvPath = "C:\Path\To\Output\UniquePermissionsReport.csv"

# Connect to the SharePoint site
Connect-PnPOnline -Url $siteUrl -UseWebLogin

# Check if the boolean column already exists; if not, create it
$columnInternalName = "HasUniquePermissions"
$columnDisplayName = "Has Unique Permissions"

$existingColumns = Get-PnPField -List $listName
if (-not $existingColumns.InternalName -contains $columnInternalName) {
    Add-PnPField -List $listName -InternalName $columnInternalName -DisplayName $columnDisplayName -Type Boolean
    Write-Host "Boolean column '$columnDisplayName' added to the list."
} else {
    Write-Host "Boolean column '$columnDisplayName' already exists."
}

# Get all list items
$items = Get-PnPListItem -List $listName -PageSize 5000 -Fields "FileRef", "FileLeafRef", "Modified", "HasUniqueRoleAssignments"

# Initialize an array to store details of items with unique permissions
$uniqueItems = @()

# Iterate through each item to check for unique permissions and update the new column
foreach ($item in $items) {
    $hasUniquePermissions = $item.HasUniqueRoleAssignments

    # Add or update the boolean column value
    Set-PnPListItem -List $listName -Identity $item.Id -Values @{$columnInternalName = $hasUniquePermissions}

    # If the item has unique permissions, add it to the array
    if ($hasUniquePermissions) {
        $uniqueItems += [PSCustomObject]@{
            Name            = $item["FileLeafRef"]
            FileRef         = $item["FileRef"]
            LastModified    = $item["Modified"]
        }
    }
}

# Export the details to a CSV file
$uniqueItems | Export-Csv -Path $outputCsvPath -NoTypeInformation -Encoding UTF8

# Check if the view already exists; if not, create it
$viewName = "Unique Permissions View"
$existingViews = Get-PnPView -List $listName
if (-not $existingViews.Title -contains $viewName) {
    Add-PnPView -List $listName -Title $viewName -Fields "FileLeafRef", "FileRef", "HasUniquePermissions", "Modified" -Query "<Where><Eq><FieldRef Name='$columnInternalName' /><Value Type='Boolean'>1</Value></Eq></Where>"
    Write-Host "View '$viewName' created to display files with unique permissions."
} else {
    Write-Host "View '$viewName' already exists."
}

# Output the result
Write-Host "Details of items with unique permissions have been updated and exported to $outputCsvPath"


```