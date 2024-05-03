---
layout: page
title: 'How to find SharePoint Online content type ID?'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-01-10'
---

What is SharePoint Online Content Type ID?

Content type IDs in SharePoint uniquely identify the content type and are designed to be recursive. The content type ID shows that content type's **lineage**, or the line of parent content types from which the content type inherits. Each SharePoint content type ID contains the ID of the parent content type, which in turn contains the ID of that content type's parent, and so on, ultimately back to and including the System content type ID. SharePoint uses this information to [determine the relationship between content types](https://learn.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/aa543822(v=office.14)?redirectedfrom=MSDN) and for push-down operations.

You can construct a valid SharePoint content type ID using one of two conventions:

* Parent content type ID + two hexadecimal values (the two hexadecimal values cannot be "00")
* Parent content type ID + "00" + hexadecimal GUID

 <img src="/articles/images/Github-AddContentType2-1.png" width="400"><br/>
<sup>Source: https://msdn.microsoft.com/en-us/library/office/aa543822%28v=office.14%29.aspx?f=255&MSPPError=-2147217396</sup>



An ID of a custom content type based on Item, will look like this when using GUID approach:
0x01 which is an Item Content Type ID
00   
9e862727eed04408b2599b25356e7914
the hexadecimal value 


Item Content Type ID	following the convention	 the hexadecimal value 
0x01	00   	9e862727eed04408b2599b25356e7914
Altogether: 0x01009e862727eed04408b2599b25356e7914
 
You can create a guid automatically using Windows Powershell 
```powershell
[System.Guid]::NewGuid().toString()
```
or
```powershell
[guid]::NewGuid()
```
You can also invent your own and using 0x0100aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa also works :)  though it's not exactly the best practice.

Each SharePoint content type ID must be unique within a SharePoint site collection. If the script does not specify the guid, SharePoint will assign one to the content type, based on the parent content type.



<h1>How to find SharePoint Online content type ID using UI</h1>

