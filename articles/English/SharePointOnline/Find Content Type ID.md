---
layout: page
title: 'SharePoint content type is still in use'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-01-10'
---

How to find SharePoint Online Content Type ID?

Content Type ID

Content type IDs uniquely identify the content type and are designed to be recursive. The content type ID encapsulates that content type's lineage, or the line of parent content types from which the content type inherits. Each content type ID contains the ID of the parent content type, which in turn contains the ID of that content type's parent, and so on, ultimately back to and including the System content type ID. SharePoint Foundation uses this information to determine the relationship between content types and for push-down operations.[1]

You can construct a valid content type ID using one of two conventions:

Parent content type ID + two hexadecimal values (the two hexadecimal values cannot be "00")
Parent content type ID + "00" + hexadecimal GUID

Source: https://msdn.microsoft.com/en-us/library/office/aa543822%28v=office.14%29.aspx?f=255&MSPPError=-2147217396

An ID of a custom content type based on Item, will look like this when using GUID approach:
0x01 which is an Item Content Type ID
00   
9e862727eed04408b2599b25356e7914
the hexadecimal value 


Item Content Type ID	following the convention	 the hexadecimal value 
0x01	00   	9e862727eed04408b2599b25356e7914
Altogether: 0x01009e862727eed04408b2599b25356e7914
​
You can create a guid automatically using Windows Powershell 
```powershell
[System.Guid]::NewGuid().toString()
```
or
```powershell
[guid]::NewGuid()
```

You can also invent your own and the following script will be using 0x0100aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa which also works :)

Each content type ID must be unique within a site collection. If the script does not specify the guid, SharePoint will assign one to the content type.

ParentContentType

Parent Content Type specifies the parent content type for the content type that will be constructed. The parameter can use both get; and set; methods. ParentContentType parameter is mutually exclusive with the ID parameter. It is due to the information encoded in the ID. ID parameter itself includes the parent content type in its first characters (e.g. "0x01" for the item). So what happens if I will try to create a content type with both those parameters specified?

$lci.ID="0x0108009e862727eed04408b2599b25356e7914"
$lci.ParentContentType=$ctx.Web.ContentTypes.GetById("0x01")

"Parameters.Id, parameters.ParentContentType cannot be used together. Please only use one of them."


