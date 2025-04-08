---
layout: page
title: 'Approve site pages using PnP'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-04-03'
---


Especially useful if you need to approve a lot of files in a batch. Works for approving SharePoint pages, news items, or library files.


# Get pages

First let us get the pages from the library and view their approval status.

```powershell

Connect-PnPOnline -Url https://yourtenant.sharepoint.com/sites/yoursite -Interactive -ClientId "b8d9-yourappid-709de"

Get-PnPListItem -List "Site Pages" | Select-Object @{Name="FileRef";Expression={$_.FieldValues["FileRef"]}}, 
                                          @{Name="_ModerationStatus";Expression={$_.FieldValues["_ModerationStatus"]}}, 
                                          @{Name="PromotedState";Expression={$_.FieldValues["PromotedState"]}}, Id

```


<br/>

# Approve a page

In order to approve a news or a page in SharePoint, you need to set _ModerationStatus property to 0.

```powershell

Set-PnPListItem -List "Site Pages" -Identity 1090 -Values @{"_ModerationStatus" = "0"}
```

There are 4 values for the Moderation Status:

0 - Approved

1 - Rejected

2 - Pending

3 - Draft

<br/>

# Approve all pages

```powershell
# Connect to SharePoint
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive  -ClientId "b8d9-yourappid-709de"

# Get all items from the "Site Pages" library
$pages = Get-PnPListItem -List "Site Pages"

# Loop through each item and approve it
foreach ($page in $pages) {
    $itemId = $page.Id
    Write-Host "Approving page with ID: $itemId"
    
    Set-PnPListItem -List "Site Pages" -Identity $itemId -Values @{"_ModerationStatus" = "0"}
}

Write-Host "All pages have been approved"
```

<br/>

# Reject a page

In order to reject a news or a page in SharePoint, you need to set _ModerationStatus property to 1. 

```powershell

Set-PnPListItem -List "Site Pages" -Identity 1090 -Values @{"_ModerationStatus" = "1"}
```


<br/>


# Reject all pages


```powershell

# Connect to SharePoint
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Get all items from the "Site Pages" library
$pages = Get-PnPListItem -List "Site Pages" -Fields "FileLeafRef"

# Loop through each item and reject it
foreach ($page in $pages) {
    $itemId = $page.Id
    $fileName = $page.FieldValues["FileLeafRef"]
    
    Write-Host "Rejecting page: $fileName (ID: $itemId)"
    
    Set-PnPListItem -List "Site Pages" -Identity $itemId -Values @{"_ModerationStatus" = "1"}
}

Write-Host "All pages have been rejected"

```

<br/>

# See Also

[Set-PnPListItem](https://pnp.github.io/powershell/cmdlets/Set-PnPListItem.html)


<!-- Default Statcounter code for spo - sitepagesAll
https://powershellscripts.github.io/articles/en/spo/approvefiles
-->
<script type="text/javascript">
var sc_project=13111303; 
var sc_invisible=1; 
var sc_security="cef0a9f7"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="real time web
analytics" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/13111303/0/cef0a9f7/1/"
alt="real time web analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->