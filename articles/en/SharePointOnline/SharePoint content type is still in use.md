---
layout: page
title: 'SharePoint content type is still in use'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-01-01'
---

<h1>Issue description</h1>
You navigate to SharePoint List -> List Settings -> Scroll down to content types -> Open a content type
 
<img src="/articles/images/Github-CTinUse1.png" width="400">
Click delete this content type and receive the following error message:

 <img src="/articles/images/Github-CTinuse2.png" width="400">

Most likely one of the items is still the content type. The following cmdlets will show how to find and remove such items.

 


1. Download and install SharePoint Online SDK.
2. Download SPOMod available here.

3. Open SPOMod current file (in ISE or NotePad) and scroll down to the following lines:

 
```powershell
# Paths to SDK. Please verify location on your computer.
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
```

4. Make sure that the paths correspond to the locations of SDK on your machine. Most often you need to change 16 into 15.

5. Save the file.

6. Open Powershell as an Administrator.

7. Run ```Import-Module PathToTheSPOModFile```

8. ```Connect-SPOCSOM```

9. Ready to go :)

Enter the following cmdlet. It will remove all items matching specific ContentTypeID:

 
```powershell
foreach($item in(Get-SPOListItems -ListTitle YOURLISTTITLE -IncludeAllProperties $true | where {$_.ContentTypeId -match "0x0111002440A334027B18479FB4EDAFF1F149FF00AC40BD13F3F10A46A50C44E1F6D19EF0"})) { Remove-SPOListItem -ListTitle YOURLISTTITLE -ItemID $item.ID}
```

 


The cmdlet involves data loss
The items will be removed from the Recycle Bin as well! There is no way to retrieve them.

 

<h1>Alternative UI approach</h1>

Step 1. Navigate to the list. Open the ribbon and click Modify view:

 

Step 2.Content type"

 

Step 3. Save the view.

Step 4. In the list click on the content type column and select the content type you are about to delete.

 

Step 5. You will see the applied filter.

 

Step 6. Mark all the items (they all should be showing the content type you want to delete).

 

Step 7. Delete the items.

 

 
