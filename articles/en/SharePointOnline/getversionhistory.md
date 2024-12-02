---
layout: page
title: 'Export file version history'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-08-11'
---


# Overview

Versioning in SharePoint Online is available for list items in all default SharePoint Online list types—including calendars, issue tracking lists, and custom lists. It is also available for all file types that can be stored in libraries, including Web Part pages. 
You can export SharePoint file version history for record-keeping purposes or compliance reasons. 

### When versions are created

There are several actions that trigger version creation. When versioning is enabled, versions are created in the following situations:

* When a list item or SharePoint file is first created or when a file is uploaded. Note that if you have enabled a library setting that requires file checkout, you have to check the file in to create its first version.

* When a file is uploaded that has the same name as an existing file and the Add as a new version to existing files check box is selected.

* When the properties of a list item or file are changed.

* When a file is opened, edited, and saved. A version is created when you first click Save. It retains the same version number for the duration of the current editing session. Even though you might save it several times, the version remains the same. When you close it and then reopen it for another editing session, another version is created.

* During co-authoring of a document, when a different user begins working on the document or when a user clicks save to upload changes to the library. The default time period for creating new versions during co-authoring is 30 minutes. This is configurable per web application in SharePoint on-premises. This setting is not configurable in SharePoint Online.

Based on: [How does versioning work in a list or library?](https://support.microsoft.com/en-us/office/how-versioning-works-in-lists-and-libraries-0f6cd105-974f-44a4-aadb-43ac5bdfd247#:~:text=When%20versioning%20is%20enabled%20in,content%20posted%20on%20your%20site.) 

<br/><br/>

# Export version count for all files in one SharePoint Online library

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

# Get version history for a single SharePoint file

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


# Export version history for all files in a library

Get version history for each SharePoint files and export it to a csv file. The cmdlets loop through all the files in a SharePoint library.

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

### [Get Version History on GitHub](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Lists%20and%20Libraries%20Management/Versioning)

[Create a report on all file versions in the library](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Lists%20and%20Libraries%20Management/Versioning/Create%20a%20report%20on%20all%20file%20versions%20in%20the%20library)

[Create a report on file versions in library or folder](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Lists%20and%20Libraries%20Management/Versioning/Create%20a%20report%20on%20file%20versions%20in%20library%20or%20folder)

[Delete all previous file versions in a library](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Lists%20and%20Libraries%20Management/Versioning/Delete%20all%20previous%20file%20versions%20in%20a%20library)

[Enable minor and major versions for all lists in one site](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Lists%20and%20Libraries%20Management/Versioning/Enable%20minor%20and%20major%20versions%20for%20all%20lists%20in%20one%20site)

[Enable minor versions using Powershell and CSOM](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Lists%20and%20Libraries%20Management/Versioning/Enable%20minor%20versions%20using%20Powershell%20and%20CSOM)

[Enable versioning for all lists in one site](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Lists%20and%20Libraries%20Management/Versioning/Enable%20versioning%20for%20all%20lists%20in%20one%20site)

[Enable versioning for one list](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Lists%20and%20Libraries%20Management/Versioning/Enable%20versioning%20for%20one%20list)

[Get versioning settings for all lists](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Lists%20and%20Libraries%20Management/Versioning/Get%20versioning%20settings%20for%20all%20lists)



<!-- Default Statcounter code for SPO getversionhistory
https://powershellscripts.github.io/articles/en/SharePointOnline/getversionhistory/
-->
<script type="text/javascript">
var sc_project=13065139; 
var sc_invisible=1; 
var sc_security="25edbfe7"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13065139/0/25edbfe7/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->