---
layout: page
title: 'Update SharePoint folder without creating a new version'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-08-03'
---

# Introduction

PnP Powershell module allows you now to update your SharePoint Online folders without creating a new version or triggering a workflow.


# Update a folder

Using the following code you can update a folder without changing the Modified field and without triggering a workflow.

```powershell
# Connect to the SharePoint site
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Get the folder you want to update
$folder = Get-PnPFolder -Url "/sites/yoursite/Shared Documents/YourFolder"

# Update the folder's metadata (e.g., Title or any custom metadata)
Set-PnPListItem -List "Shared Documents" -Identity $folder.ListItemAllFields.Id -Values @{"Title"="New Folder Title"} -UpdateType UpdateOverwriteVersion
```

<br/><br/>

Using the following code you can update a folder without changing the Modified field. It triggers any associated workflow or event.

```powershell
# Connect to the SharePoint site
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Get the folder you want to update
$folder = Get-PnPFolder -Url "/sites/yoursite/Shared Documents/YourFolder"

# Update the folder's metadata (e.g., Title or any custom metadata)
Set-PnPListItem -List "Shared Documents" -Identity $folder.ListItemAllFields.Id -Values @{"Title"="New Folder Title"} -UpdateType SystemUpdate
```


# Update folder permissions

Using PowerShell and the PnP.PowerShell module, you can update a folder's permissions without creating a new version by using the `Set-PnPFolderPermission` cmdlet with the `[-SystemUpdate]` parameter.

<img src="/articles/images/systemupdate2.PNG">


```powershell

# Connect to the SharePoint site
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -Interactive

# Update the folder's metadata (e.g., Title or any custom metadata)
Set-PnPFolderPermission -List "Shared Documents" -Identity "/sites/yoursite/Shared Documents/YourFolder" -User "user@domain.com" -AddRole "Contribute" -SystemUpdate
```

### Explanation

`Connect-PnPOnline`: Connects to your SharePoint site. Use your site URL in the -Url parameter.

`Set-PnPFolderPermission`: This cmdlet sets permissions on the specified folder.
-List: The document library where the folder resides.
-Identity: The folder path. Replace "/sites/yoursite/Shared Documents/YourFolder" with the actual folder path.
-User: The user to whom you're assigning the permissions.
-AddRole: The role you want to assign to the user, such as "Contribute", "Read", or "Full Control".
-SystemUpdate: Ensures the update is made without creating a new version of the folder.



# See Also

[Set-PnPFolderPermission](https://pnp.github.io/powershell/cmdlets/Set-PnPFolderPermission.html)


<!-- Default Statcounter code for SystemUpdateFolder
https://powershellscripts.github.io/articles/en/SharePointOnline/systemupdatefolder/
-->
<script type="text/javascript">
var sc_project=13025466; 
var sc_invisible=1; 
var sc_security="d1a2cfd0"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13025466/0/d1a2cfd0/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->