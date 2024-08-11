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

* Update: Sets field values and creates a new version if versioning is enabled for the list. The "Modified By" and "Modified" fields will be updated to reflect the time of the update and the user who made the change.

* SystemUpdate: Sets field values and does not create a new version. Any events on the list will trigger. The "Modified By" and "Modified" fields are not updated and can not be set.

* UpdateOverwriteVersion: Sets field values and does not create a new version. No events on the list will trigger. The "Modified By" and "Modified" fields are not updated but can be set by passing the field values in the update. HINT: use 'Editor' to set the "Modified By" field.

# Update batch items


# Update item permissions


Set-PnPImageListItemColumn


# See Also
