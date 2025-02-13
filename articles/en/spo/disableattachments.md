---
layout: page
title: 'Disable attachments in SharePoint list'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-02-09'
---


# Disable attachments in your SharePoint lists

Modern SharePoint lists often allow users to attach files to list items. However, in certain scenarios, you might want to disable this feature to maintain a cleaner or more secure list environment. For example, if your list is used purely for tracking text-based data, attachments might be unnecessary or even counterproductive.

## User Interface

You can disable the attachments in your SharePoint list directly in the browser and for a single list I would recommend this approach

<iframe width="660" height="400" src="https://www.youtube.com/embed/i8In3duIdq0" frameborder="0" allowfullscreen></iframe>



## Powershell


With PowerShell and the PnP module, disabling attachments in a SharePoint list is simple. Here’s the command to achieve this:

```powershell

Set-PnPList -Identity "Demo List" -EnableAttachments $false

```

This script disables the attachment feature for the specified list, replacing "Demo List" with the name of your target list. Make sure you are connected to your SharePoint site using the Connect-PnPOnline cmdlet before running this command.



### For all lists in one site collection

```powershell

# Connect to the SharePoint site collection
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -UseWebLogin

# Get all lists in the site
$lists = Get-PnPList

# Disable attachments for all lists
foreach ($list in $lists) {
    Set-PnPList -Identity $list.Title -EnableAttachments $false
}

Write-Host "Attachments have been disabled for all lists in the site collection."

```


### For all site collections (the entire tenant)

The script below disables attachments on all lists in the entire Microsoft 365 tenant. Mind you, Teams are also SharePoint sites!

```powershell

# Connect to the SharePoint Admin Center
Connect-PnPOnline -Url "https://yourtenant-admin.sharepoint.com" -UseWebLogin

# Get all site collections in the tenant
$sites = Get-PnPTenantSite -IncludeOneDriveSites $false

foreach ($site in $sites) {
    Write-Host "Processing site: $($site.Url)"
    
    # Connect to each site
    Connect-PnPOnline -Url $site.Url -UseWebLogin
    
    # Get all lists in the site
    $lists = Get-PnPList
    
    # Disable attachments for all lists in the site
    foreach ($list in $lists) {
        if ($list.BaseTemplate -eq 100) { # Only target custom lists
            Set-PnPList -Identity $list.Title -EnableAttachments $false
            Write-Host "Disabled attachments for list: $($list.Title)"
        }
    }
}

Write-Host "Attachments have been disabled for all lists in all sites."

```


## Github

For older SharePoint versions and CSOM have a look at my other scripts at [Github](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Items%20Management/Attachments)

