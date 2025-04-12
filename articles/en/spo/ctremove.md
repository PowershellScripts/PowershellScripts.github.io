---
layout: page
title: 'Content Type is still in use - How to remove stuck site content type'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-04-05'
---

# Issue 


You navigate to SharePoint List -> List Settings -> Scroll down to content types -> Open a content type

<img src="/articles/img/ctremove0.png" width="600">


Click delete this content type and receive the following error message:

<img src="/articles/img/ctremove.png" width="600">


# Solution


The error message means that somewhere out there there are still items using that content type you are trying to delete. There are several ways you can fix it:

* User Interface (Browser)
* PNP Powershell
* CSOM + Powershell
* REST API


<br/>

# User Interface (Browser)

Step 1. Navigate to the list. Modify the view:

 <img src="/articles/img/ctinuse.png">

Step 2. Include Content type column:

 <img src="/articles/img/ctinuse1.png">

Step 3. Save the view.

Step 4. In the list click on the content type column and select the content type you are about to delete.

 
 <img src="/articles/img/ctinuse2.png">


Step 6. Either delete the items or change their content type under properties.

  <img src="/articles/img/ctinuse3.png">

 

<br/>

# Powershell

Powershell is a better choice if you don't know in which list the content type is being used and there is a lot of them. Powershell allows you to scan through multiple lists quickly.

### With content type name 
Content type name is easier because you already know the name of your content type. However, if there is a chance that you may have 2 different content types with the same name, you should be using the script with content type id, not name. For a script with content type id, scroll below.


```powershell

# Specify your site and your client ID
Connect-PnPOnline -Url "https://acco967.sharepoint.com/sites/EWZ" -Interactive  -ClientId 00000000-4ead-4642-93a9-0000000000    

# Specify the name of your content type
$ContentTypeName = "MyCustomComment"   

# Get all lists in the site
$lists = Get-PnPList

# Initialize results
$results = @()

foreach ($list in $lists) {
    # Skip hidden or system lists
    if ($list.Hidden) { 
        continue
    }

    # Get items in the list with the specified content type
    $items = Get-PnPListItem -List $list.Title -Query "<View><Query><Where><Eq><FieldRef Name='ContentType' /><Value Type='Computed'>$ContentTypeName</Value></Eq></Where></Query></View>" -PageSize 1000

    # Count the items and get their URLs
    $count = $items.Count
    if ($count -gt 0) {
        $itemUrls = $items | ForEach-Object {
            "$($_.FieldValues.FileRef)"
        }

        # Add the results to the output
        $results += [PSCustomObject]@{
            ListName  = $list.Title
            ItemCount = $count
            ItemUrls  = $itemUrls -join ", "
        }
    }
}

# Display the results
if ($results.Count -eq 0) {
    Write-Output "No items with content type '$ContentTypeName' found."
} else {
    $results | Format-Table -AutoSize
}

# Optionally export the results to a CSV file
$results | Export-Csv -Path "ContentTypeReport.csv" 

# Disconnect from SharePoint
Disconnect-PnPOnline


```





### With content type id

You can find content type ID [using browser](https://powershellscripts.github.io/articles/en/SharePointOnline/findctid/) or [Powershell](https://powershellscripts.github.io/articles/en/SharePointOnline/findctIDPS/).



```powershell

# Connect to the SharePoint site
Connect-PnPOnline -Url "https://acco967.sharepoint.com/sites/EWZ" -Interactive  -ClientId 00000000-4ead-4642-93a9-0000000000    # Specify your site and your client ID

# Define the Content Type ID to search for
$ContentTypeId = "0x0101001234567890ABCDE"

# Get all lists in the site
$lists = Get-PnPList

# Initialize results
$results = @()

foreach ($list in $lists) {
    # Skip hidden or system lists
    if ($list.Hidden) { 
        continue
    }

    # Get items in the list with the specified content type
     $items = Get-PnPListItem -List $list.Title -Query "<View><Query><Where><BeginsWith><FieldRef Name='ContentTypeId' /><Value Type='Text'>$ContentTypeId</Value></BeginsWith></Where></Query></View>" -PageSize 1000

    # Count the items and get their URLs
    $count = $items.Count
    if ($count -gt 0) {
        $itemUrls = $items | ForEach-Object {
            "$($_.FieldValues.FileRef)"
        }

        # Add the results to the output
        $results += [PSCustomObject]@{
            ListName  = $list.Title
            ItemCount = $count
            ItemUrls  = $itemUrls -join ", "
        }
    }
}

# Display the results
if ($results.Count -eq 0) {
    Write-Output "No items with content type ID '$ContentTypeId' found."
} else {
    $results | Format-Table -AutoSize
}

# Optionally export the results to a CSV file
$results | Export-Csv -Path "ContentTypeReport.csv" 

# Disconnect from SharePoint
Disconnect-PnPOnline


```

<br/>

# Read Only

If your content type is read only, you need to change that setting first.

Navigate to:

1. Site settings
2. Site content types
3. Click on the name of the content type
4. Advanced settings
5. Should this content type be read only? Select NO



<br/>

# See Also

[List SharePoint content types](https://powershellscripts.github.io/articles/en/spo/ctget/)





<!-- Default Statcounter code for SPO ctremove
https://powershellscripts.github.io/articles/en/spo/ctremove/
-->
<script type="text/javascript">
var sc_project=13116809; 
var sc_invisible=1; 
var sc_security="0782bf04"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/13116809/0/0782bf04/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->