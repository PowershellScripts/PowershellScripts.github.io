---
layout: page
title: 'Get version history programmatically'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-08-11'
---


# Overview

Versioning in SharePoint Online is available for list items in all default SharePoint Online list types—including calendars, issue tracking lists, and custom lists. It is also available for all file types that can be stored in libraries, including Web Part pages. 

### When versions are created

There are several actions that trigger version creation. When versioning is enabled, versions are created in the following situations:

* When a list item or SharePoint file is first created or when a file is uploaded. Note that if you have enabled a library setting that requires file checkout, you have to check the file in to create its first version.

* When a file is uploaded that has the same name as an existing file and the Add as a new version to existing files check box is selected.

* When the properties of a list item or file are changed.

* When a file is opened, edited, and saved. A version is created when you first click Save. It retains the same version number for the duration of the current editing session. Even though you might save it several times, the version remains the same. When you close it and then reopen it for another editing session, another version is created.

* During co-authoring of a document, when a different user begins working on the document or when a user clicks save to upload changes to the library. The default time period for creating new versions during co-authoring is 30 minutes. This is configurable per web application in SharePoint on-premises. This setting is not configurable in SharePoint Online.

Based on: [How does versioning work in a list or library?](https://support.microsoft.com/en-us/office/how-versioning-works-in-lists-and-libraries-0f6cd105-974f-44a4-aadb-43ac5bdfd247#:~:text=When%20versioning%20is%20enabled%20in,content%20posted%20on%20your%20site.) 

<br/><br/>

# Retrieve versions for all files in one SharePoint Online library

The following PnP Powershell cmdlets can help you retrieve the number of versions for each SharePoint file.

```powershell
# Define the document library name
$libraryName = "Shared Documents"  # Replace with your document library name

# Define the path for the CSV output
$outputCsvPath = "C:\path\to\output\file_versions_report.csv"  # Replace with your desired output path

# Get all files in the document library
$files = Get-PnPListItem -List $libraryName -PageSize 5000 | Where-Object { $_.FileSystemObjectType -eq "File" }

# Prepare an array to hold the results
$results = @()

# Iterate through each file to get version information
foreach ($file in $files) {
    # Get the file object by its server relative URL
    $fileObj = Get-PnPFile -Url $file["FileRef"] -AsFile

    # Get the versions of the file
    $versions = Get-PnPProperty -ClientObject $fileObj -Property Versions

    # Create an object with the file name, path, and number of versions
    $result = [PSCustomObject]@{
        FileName    = $file["FileLeafRef"]
        FilePath    = $file["FileRef"]
        VersionCount = $versions.Count
    }

    # Add the result to the results array
    $results += $result
}

# Export results to CSV
$results | Export-Csv -Path $outputCsvPath -NoTypeInformation
```

<br/><br/>

# Get version history for a single file

Get the version history for a single SharePoint file programmatically, using PnP. You need the file path. The cmdlets export the version history to a csv file.

```powershell

# Define the file URL (relative to the site)
$fileUrl = "/sites/yoursite/Shared Documents/YourFolder/YourFileName.docx" 

# Get the file object
$file = Get-PnPFile -Url $fileUrl -AsFile

# Get the versions of the file
$versions = Get-PnPProperty -ClientObject $file -Property Versions

# Prepare an array to hold the results
$results = @()

# Iterate through each version and extract details
foreach ($version in $versions) {
    $result = [PSCustomObject]@{
        VersionNumber = $version.VersionLabel
        ModifiedBy    = $version.CreatedBy.Email  # Use DisplayName for the user's full name
        ModifiedDate  = $version.Created
        Comments      = $version.CheckInComment
    }

    $results += $result
}

# Output the version history
$results | Format-Table -AutoSize


$outputCsvPath = "C:\path\to\output\file_version_history.csv"  
$results | Export-Csv -Path $outputCsvPath -NoTypeInformation
```


# Get version history for all files in a library

Get version history and export it to a csv file. The cmdlets loop through all the files in a library.

```powershell

$libraryName = "Shared Documents"  

# Get all files in the document library
$files = Get-PnPListItem -List $libraryName -PageSize 5000 | Where-Object { $_.FileSystemObjectType -eq "File" }

$results = @()

# Iterate through each file to get version information
foreach ($file in $files) {
    # Get the file object by its server relative URL
    $fileObj = Get-PnPFile -Url $file["FileRef"] -AsFile

    # Get the versions of the file
    $versions = Get-PnPProperty -ClientObject $fileObj -Property Versions

    # Iterate through each version and extract details
    foreach ($version in $versions) {
        $result = [PSCustomObject]@{
            FileName      = $file["FileLeafRef"]
            FilePath      = $file["FileRef"]
            VersionNumber = $version.VersionLabel
            ModifiedBy    = $version.CreatedBy.Email  # Use DisplayName for the user's full name
            ModifiedDate  = $version.Created
            Comments      = $version.CheckInComment
        }

        $results += $result
    }
}

# Output the version history - it might be overwhleming on a single screen!
$results | Format-Table -AutoSize

# Export results to CSV
$outputCsvPath = "C:\path\to\output\file_version_history_report.csv"  
$results | Export-Csv -Path $outputCsvPath -NoTypeInformation

```


# See Also

Get Version History on GitHub


