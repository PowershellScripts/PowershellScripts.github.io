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

PnP Powershell module allows you now to update your SharePoint Online list items without creating a new version and without triggering a workflow. The item is updated without changing the modified date.


# Update an item

### UpdateType

The new UpdateType parameter in Set-PnpListItem allows for three options:

* **Update**: Sets field values and creates a new version if versioning is enabled for the list. The "Modified By" and "Modified" fields will be updated to reflect the time of the update and the user who made the change.

* **SystemUpdate**: Sets field values and does not create a new version. Any events on the list will trigger. The "Modified By" and "Modified" fields are not updated and can not be set.

* **UpdateOverwriteVersion**: Sets field values and does not create a new version. No events on the list will trigger. The "Modified By" and "Modified" fields are not updated but can be set by passing the field values in the update. HINT: use 'Editor' to set the "Modified By" field.


## Example 1: Updating a List Item Without Updating the Modfied Date Or Triggering a Workflow
Using the Set-PnPListItem cmdlet, you can update a list item and overwrite the current version without modifying the Modified or Modified By fields. Any workflows (e.g. Power Automate) or events will not trigger. Here’s how you can do it:

```powershell
Set-PnPListItem -List "Demo List" -Identity 1 -Values @{"Editor"="testuser@domain.com"} -UpdateType UpdateOverwriteVersion
```

Explanation:

-List "Demo List": Specifies the name of the list where the item resides.
-Identity 1: List item ID.
-Values @{"Editor"="testuser@domain.com"}: Specifies the fields to be updated. In this example, we are updating the "Editor" field.
-UpdateType UpdateOverwriteVersion: This parameter ensures that the update is applied to the existing version of the item without creating a new version.
When to Use UpdateOverwriteVersion


### Example 2: Updating the Title Field

```powershell
Set-PnPListItem -List "Demo List" -Identity 1 -Values @{"Title"="New Title"} -UpdateType SystemUpdate
```

This command updates the "Title" field of the item without changing the "Modified" date.

### Example 3: Updating the Due Date Field

```powershell
Set-PnPListItem -List "Demo List" -Identity 1 -Values @{"DueDate"="2024-12-31"} -UpdateType SystemUpdate
```

Here, the "DueDate" field is updated without altering the "Modified" date.



### Example 4: Updating Multiple Fields Simultaneously

```powershell
Set-PnPListItem -List "Demo List" -Identity 1 -Values @{"Title"="Updated Title"; "Status"="Completed"; "Priority"="High"} -UpdateType SystemUpdate
```

This command updates the "Title," "Status," and "Priority" fields all at once, again without modifying the "Modified" date.



## Example 5: Updating a List Item Without Updating the Modfied Date But Triggering a Workflow
When working with SharePoint lists, there may be scenarios where you need to update an item without altering its "Modified" date. This can be important when you want to make background updates to items without signaling to users that a change has occurred, while still triggering associated events or workflows like Power Automate.

Using the Set-PnPListItem cmdlet, you can update a list item without changing its "Modified" date but still trigger workflows. This can be done using the SystemUpdate option

````powershell
Set-PnPListItem -List "Demo List" -Identity 1 -Values @{"Editor"="testuser@domain.com"} -UpdateType SystemUpate
```

Explanation:

-List "Demo List": Specifies the name of the list where the item resides.
-Identity 1: List item ID.
-Values @{"Editor"="testuser@domain.com"}: Specifies the fields to be updated. In this example, we are updating the "Editor" field.
-UpdateType UpdateOverwriteVersion: This parameter ensures that the update is applied to the existing version of the item without creating a new version.
When to Use UpdateOverwriteVersion


# Update batch items


# Update item permissions




Set-PnPImageListItemColumn

# Important Considerations
Auditability: Be cautious when using this approach, as it overwrites the current version, potentially losing track of changes that might be important for auditing purposes.
User Awareness: Ensure that users are aware of when and why this method is used, as it deviates from the usual behavior of versioning in SharePoint.


# See Also

[Update SharePoint folder without creating a new version](https://powershellscripts.github.io/articles/en/SharePointOnline/systemupdatefolder)