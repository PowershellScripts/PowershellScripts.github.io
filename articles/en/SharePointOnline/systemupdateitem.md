---
layout: page
title: 'Update SharePoint list item without changing the modified date'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-08-03'
---

# Introduction
When working with SharePoint lists, you often need to update items. By default, each update to a list item creates a new version, which is useful for tracking changes but in some cases, you may want to update a list item without creating a new version. This is where the UpdateOverwriteVersion and SystemUpdate options in PnP module becomes valuable.

PnP Powershell module allows you now to update your SharePoint Online list items without creating a new version and without triggering a workflow. The item is updated without changing the modified date. The version number is not increased.


# Update an item

### UpdateType

The new UpdateType parameter in Set-PnpListItem allows for three options:

* **Update**: Sets field values and creates a new version if versioning is enabled for the list. The "Modified By" and "Modified" fields will be updated to reflect the time of the update and the user who made the change.

* **SystemUpdate**: Sets field values and does not create a new version. Any events on the list will trigger. The "Modified By" and "Modified" fields are not updated and can not be set.

* **UpdateOverwriteVersion**: Sets field values and does not create a new version. No events on the list will trigger. The "Modified By" and "Modified" fields are not updated but can be set by passing the field values in the update. HINT: use 'Editor' to set the "Modified By" field. 

<br/>

### Example 1
##### Updating a List Item Without Updating the Modfied Date Or Triggering a Workflow
Using the Set-PnPListItem cmdlet, you can update a list item and overwrite the current version without modifying the Modified or Modified By fields. Any workflows (e.g. Power Automate) or events will not trigger. Here’s how you can do it:

```powershell
Set-PnPListItem -List "Demo List" -Identity 1 -Values @{"Editor"="testuser@domain.com"} -UpdateType UpdateOverwriteVersion
```

**Explanation**:

-List "Demo List": Specifies the name of the list where the item resides.

-Identity 1: List item ID.

-Values @{"Editor"="testuser@domain.com"}: Specifies the fields to be updated. In this example, we are updating the "Editor" field.

-UpdateType UpdateOverwriteVersion: This parameter ensures that the update is applied to the existing version of the item without creating a new version.


<br/>

### Example 2
###### Updating the Title Field

```powershell
Set-PnPListItem -List "Demo List" -Identity 1 -Values @{"Title"="New Title"} -UpdateType UpdateOverwriteVersion
```

This command updates the "Title" field of the item without changing the "Modified" date.

<br/>

### Example 3
###### Updating the Due Date Field

```powershell
Set-PnPListItem -List "Demo List" -Identity 1 -Values @{"DueDate"="2024-12-31"} -UpdateType UpdateOverwriteVersion
```

Here, the "DueDate" field is updated without altering the "Modified" date.


<br/>

### Example 4
###### Updating Multiple Fields Simultaneously

```powershell
Set-PnPListItem -List "Demo List" -Identity 1 -Values @{"Title"="Updated Title"; "Status"="Completed"; "Priority"="High"} -UpdateType UpdateOverwriteVersion
```

This command updates the "Title," "Status," and "Priority" fields all at once, again without modifying the "Modified" date.


<br/>

## Example 5
###### Updating a List Item Without Updating the Modfied Date But Triggering a Workflow

When working with SharePoint lists, there may be scenarios where you need to update an item without altering its "Modified" date. This can be important when you want to make background updates to items without signaling to users that a change has occurred, while still triggering associated events or workflows like Power Automate.

Using the Set-PnPListItem cmdlet, you can update a list item without changing its "Modified" date but still trigger workflows. This can be done using the SystemUpdate option

```powershell
Set-PnPListItem -List "Demo List" -Identity 1 -Values @{"Editor"="testuser@domain.com"} -UpdateType SystemUpate
```

###### Explanation:

-List "Demo List": Specifies the name of the list where the item resides.

-Identity 1: List item ID.

-Values @{"Editor"="testuser@domain.com"}: Specifies the fields to be updated. In this example, we are updating the "Editor" field.

-UpdateType SystemUpdate: This parameter ensures that the update is applied to the existing version of the item without creating a new version. The event or workflow associated with the item is triggered, though.


<br/><br/><br/>

# Update batch items

If you need to update multiple list items in one go, you can do so by combining the `Set-PnPListItem` and `New-PnPBatch` cmdlets. Here's an example of how to batch update several items:

```powershell
$batch = New-PnPBatch


$items = Get-PnPListItem -List "Demo List" -PageSize 1000

foreach ($item in $items) {
    Set-PnPListItem -List "Demo List" -Identity $item.Id -Values @{"Status"="Archived"; "Reviewed"="Yes"} -UpdateType SystemUpdate -Batch $batch
}

Invoke-PnPBatch -Batch $batch
```

###### Explanation:

`New-PnPBatch` : Creates a new batch

`Get-PnPListItem -List "Demo List" -PageSize 1000`: Retrieves all items from the "Demo List." The -PageSize parameter determines how many items are retrieved per batch.

`foreach ($item in $items)`: Loops through each item in the list.

`Set-PnPListItem -List "Demo List" -Identity $item.Id -Values @{"Status"="Archived"; "Reviewed"="Yes"} -UpdateType SystemUpdate -Batch $batch`: For each item, this command updates the "Status" to "Archived" and the "Reviewed" field to "Yes," without changing the "Modified" date or creating a new version.


<br/><br/><br/>

# Set-PnPImageListItemColumn
Image column can be updated using `Set-PnPImageListItemColumn`. The cmdlet also takes [-UpdateType <UpdateType>] parameter.


### Example 1
Using the following example you can set the image value in the field without updating the Modified Date, the Modified By, or triggering a workflow.

```powershell
Set-PnPImageListItemColumn -List "Demo List" -Identity 1 -Field "Thumbnail" -ServerRelativePath "/sites/contoso/SiteAssets/test.png" -UpdateType UpdateOverwriteVersion`
```

### Example 2
Using the following example you can set the image value in the field and trigger the workflow or event associated with the list item. The Modified Date and the Modified By will not be updated.

```powershell
Set-PnPImageListItemColumn -List "Demo List" -Identity 1 -Field "Thumbnail" -ServerRelativePath "/sites/contoso/SiteAssets/test.png" -UpdateType SystemUpdate`
```

<br/><br/><br/>

# Update item permissions
You can also update item permissions without creating a new version and without updating the modified date. Use `-SystemUpdate` parameter.

<img src="/articles/images/systemupdate1.PNG">

<sup>From: https://pnp.github.io/powershell/cmdlets/Set-PnPListItemPermission.html </sup>

```powershell
Set-PnPListItemPermission -List 'Documents' -Identity 1 -AddRole 'Read' -RemoveRole 'Contribute' -Group "Site collection Visitors" -SystemUpdate
```

<br/><br/><br/>

# Important Considerations

Auditability: 

Be cautious when using this approach, as it overwrites the current version, potentially losing track of changes that might be important for auditing purposes.

User Awareness: 

Ensure that users are aware of when and why this method is used, as it deviates from the usual behavior of versioning in SharePoint.

<br/><br/><br/>

# See Also

[Update SharePoint folder without creating a new version](https://powershellscripts.github.io/articles/en/SharePointOnline/systemupdatefolder)

<!-- Default Statcounter code for SystemUpdateItem
https://powershellscripts.github.io/articles/en/SharePointOnline/systemupdateitem/
-->
<script type="text/javascript">
var sc_project=13025465; 
var sc_invisible=1; 
var sc_security="dde9ecaa"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="site stats"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13025465/0/dde9ecaa/1/"
alt="site stats"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->