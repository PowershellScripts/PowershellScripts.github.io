---
layout: page
title: 'Add SharePoint site permissions to a group using PnP'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-08-01'
---


# Introduction
Using the PnP PowerShell module, you can assign permissions to an existing group within the Microsoft 365 environment. It can be a SharePoint group, security group or Microsoft 365 group. Keep in mind that claims can make it a bit complex. For a detailed explanation, have a look at the Claims Deep Dive article.

<br/>

# Get available roles

Use `Get-PnPRoleDefinition` to retrieve all available roles you can add or remove on a SharePoint Online site.

Get-PnPRoleDefinition | select name, RoleTypeKind, description

<br/><br/>

# For SharePoint Online list


### To a SharePoint group
The following cmdlet adds permissions to an existing SharePoint group. You can use the group name directly. In case you would like to break the permission inheritance on a list from its parent, you can use [Set-PnPList -BreakRoleInheritance](Set-PnPList.md#-breakroleinheritance).

```powershell
Set-PnPListPermission -Identity test1 -User "MyGroupName" -AddRole 'Contribute'
```

<br/>

### To a security group
The following cmdlet adds permissions to an existing security group. You need to use the group ID.

```powershell
Set-PnPListPermission -Identity test1 -User "c:0t.c|tenant|6510e196-d412-41de-a2e3-f99e8c0ffb4a" -AddRole 'Contribute'
```

You can retrieve the security group ID by going to Entra ID >> Groups 

<img src="/articles/images/addpermtogroup.png"><br/>

or by using `Get-PnPAzureADGroup` cmdlet

<img src="/articles/images/addpermtogroup2.png"><br/>

<br/>

### To Microsoft 365 group
The following cmdlet adds permissions to the members of an existing Microsoft 365 group. You need to use the group ID. You can retrieve the Microsoft 365 group ID by going to Entra ID >> Groups  or by using `Get-PnPAzureADGroup` cmdlet.

```powershell
Set-PnPListPermission -Identity test1 -User "c:0o.c|federateddirectoryclaimprovider|2b7a7a59-7c52-4e42-a8f9-0675fe1ab62a" -AddRole 'Contribute'
```

Some examples how to retrieve a Microsoft 365 group using PnP cmdlets:

* ```Get-PnPAzureADGroup | where {$_.GroupTypes -eq "Unified"}```
* ```Get-PnPAzureADGroup | where {$_.DisplayName -eq "test"}```


<br/>

### To Everyone group
Everyone group is special. Use `c:0(.s|true`.

```powershell
Set-PnPListPermission -Identity test1 -User "c:0(.s|true" -AddRole 'Contribute'
```

<br/>

### To Everyone Except External users

Everyone except external users group also has its special naming. Use `c:0-.f|rolemanager|spo-grid-all-users/7110ff4b-10a6-4895-9169-2237e60672c7` for Everyone except external users. Keep in mind, that your Everyone except external users will have a different ID.

```powershell
Set-PnPListPermission -Identity test1 -User "c:0(.s|true" -AddRole 'Contribute'
```

One of the places where you can see the ID of your Everyone except external users group is SharePoint Online Site permissions.

 <img src="/articles/images/addpermtogroup5.png"><br/>
 
<br/><br/><br/>


# For SharePoint Online web (site collection)

### To a SharePoint group
The following cmdlet adds permissions to an existing SharePoint group. You can use the group name directly.

```powershell
Set-PnPWebPermission
```

<br/>

### To security group

```powershell
Set-PnPWebPermission  -User "c:0t.c|tenant|6510e196-d412-41de-a2e3-f99e8c0ffb4a" -AddRole 'Contribute'
```

<br/>

### To Microsoft 365 group members

```powershell
Set-PnPWebPermission -User "c:0o.c|federateddirectoryclaimprovider|2b7a7a59-7c52-4e42-a8f9-0675fe1ab62a" -AddRole 'Contribute'
```

<br/>

### To Everyone group

```powershell
Set-PnPWebPermission -User "c:0o.c|federateddirectoryclaimprovider|2b7a7a59-7c52-4e42-a8f9-0675fe1ab62a" -AddRole 'Contribute'
```

<br/>

### To Everyone Except External users

```powershell
Set-PnPWebPermission -User "c:0o.c|federateddirectoryclaimprovider|2b7a7a59-7c52-4e42-a8f9-0675fe1ab62a" -AddRole 'Contribute'
```

<br/><br/><br/>

# See Also

[Mike Smith's SharePoint 2013 and SharePoint Online Built-In Accounts ](https://techtrainingnotes.blogspot.com/2016/03/sharepoint-2013-and-sharepoint-online.html)

[Understanding login name format of SharePoint](https://learn.microsoft.com/en-us/answers/questions/349797/understanding-login-name-format-of-sharepoint)
