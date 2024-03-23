---
layout: page
title: 'Easy way to create CAML Query for list view'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2023-12-30'
---


<h1>CamlQuery</h1>
When loading SharePoint list items, you may want to limit the number of retrieved results. If your list contains 10 elements, then loading them like this will present no issues. 

```
CamlQuery camlQuery = new CamlQuery();
camlQuery.ViewXml = "<View Scope=\"RecursiveAll\"></View>";
ListItemCollection collListItem = list.GetItems(camlQuery);
clientContext.Load(collListItem);
clientContext.ExecuteQuery();
```

However, if your list exceeds thousands of items, loading all of them in such manner will be highly inefficient, if you need only few items that fulfill specific requirement. The ```GetItems(CamlQuery)``` method allows you to define a Collaborative Application Markup Language (CAML) query that specifies which items to return. You can pass an undefined CamlQuery object to return all items from the list, as in the example above, or use the ViewXml property to define a CAML query and return items that meet specific criteria.

The Where clause translates into the SQL SELECT statement. The format of the <Where> clause is a structured XML tree with a mixture of logical joins (<And>, <Or>), comparison operators, such as <Contains>, <Geq>, <Lt>, simple arithmetic operators, field (column) references, constant values, and predefined Collaborative Application Markup Language (CAML) constants. For a full list you can refer to Query Schema Reference in Microsoft Docs. Fields referenced in a Where element do not have to be fields of the primary list that is being queried. If another list is being joined, then fields from the foreign list can be itemized in a ProjectedFields element and can then be referenced in the <Where> element.


```
<Query>
  <Where>
    <Geq>
      <FieldRef Name="Expires"/>
      <Value Type="DateTime">
        <Today/>
      </Value>
    </Geq>
  </Where>
  <OrderBy>
    <FieldRef Name="Modified"/>
  </OrderBy>
</Query>
```

Source:  /en-us/previous-versions/office/developer/sharepoint-2010/ms414805%28v%3doffice.14%29

Step 1: Create in GUI
While a simple query is often more easily typed directly, writing CamlQuery in a complex scenario may prove to be time-consuming and prone to error. A way around it is to use GUI and create a specific view in SharePoint list/library:

                     

You can set the view to be private so that your users do not see this temporary view.

Step 2: Extract the CamlQuery
Now that the view is created and you see in the list that only items fulfilling your criteria are displayed, proceed to extract the ViewXML.

Using REST Endpoint
Find list and view's GUID. Open the created list view in edit mode and copy the url:

https://tenant.sharepoint.com/\_layouts/15/ViewEdit.aspx?**List=%7B4E998310-BCE0-4647-BB77-0183EA3E48A8%7D**&**View=%7B47FA121F-B26F-4CCD-B785-70DF105597F7%7D**&Source=%252Fsites%252FtestFlow%252F%255Flayouts%252F15%252Flistedit%252Easpx%253FList%253D%25257B4e998310%252Dbce0%252D4647%252Dbb77%252D0183ea3e48a8%25257D

List GUID is after List= , and marked in green in the example above . %7B is url encoded opening parentheses { and %7D is URL-encoded closing parentheses** } . **View GUID is after View= and marked in orange in the example above.

In your browser enter 

Copy
https://TENANT.sharepoint.com/sites/SITENAME/_api/web/lists/getbyid('4E998310-BCE0-4647-BB77-0183EA3E48A8')/Views/getbyid('47FA121F-B26F-4CCD-B785-70DF105597F7')
replacing your tenant and site collection names, the list GUID and view GUID with values from the view created in the previous point. I like to use Internet Explorer for this step, because it renders a very nice and readable XML:


Take the value from ViewQuery. 
In this example it is:

Copy
<OrderBy><FieldRef Name="Modified" Ascending="FALSE" /></OrderBy><Where><Eq><FieldRef Name="Editor" /><Value Type="User">FR</Value></Eq></Where>
Using Powershell
SOM

Load SharePoint Online SDKs. The current version at the time of writing this article and used in the example is available at  NuGet gallery.

Copy
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
Create SharePoint context and assign credentials of a user who has enough access to the list and its views:

Copy
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
Load the list and its views:

Copy
$ll=$ctx.Web.Lists.GetByTitle($ListTitle)
$ctx.load($ll)
$ctx.load($ll.Views)
$ctx.ExecuteQuery()
Load the view and ViewFields. If you do not load the ViewFields, you will still be able to access the view properties, but not output the entire $view object:

Copy
$view = $ll.Views.GetById($ViewGUID)
$ctx.Load($view)
$ctx.Load($view.ViewFields)
$ctx.ExecuteQuery()
If you want to use view's name instead of its GUID, use .GetByTitle method. Instead of the lines above, use:

Copy
$view = $ll.Views.GetByTitle($ViewTitle)
$ctx.Load($view)
$ctx.Load($view.ViewFields)
$ctx.ExecuteQuery()
Display the view and its properties:

Copy
Write-Output $view
or export it to CSV using:


Copy
Export-CSV $view
 

SPOMod
SPOMod is a custom Powershell module for SharePoint Online and SharePoint Server available for download at Technet Gallery and GitHub. Full list of cmdlets is available at Social Technet and in case of issues, there is an Installation Guide. 
Using SPOMod allows you to get all list properties in just 3 lines of script. 

Connect to your site collection or subsite 
using Connect-SPOCSOM if you are in SharePoint Online environment and Connect-SPCSOM if you are on SharePoint Server 2013/2016/2019. 

Copy
$creds = Get-Credential
Connect-SPOCSOM -Credential $creds -Url $url
Get all views and export their properties to CSV file:

Copy
Get-SPOListView -ListName MyList1 -IncludeAllProperties | export-csv c:\CSVPathFile.csv
Remember to substitute your list name and the correct path to where you want your csv file created.

OR
Get properties of a specific view using its name:


Copy
Get-SPOListView -ListName MyList1 -IncludeAllProperties | where{$_.Title -eq "FR"}
Substitute the values in green with your list name and the name of the view

OR
Get properties of a specific view using its GUID:


Copy
Get-SPOListView -ListName MyList1 -IncludeAllProperties | where{$_.ID -eq "47fa121f-b26f-4ccd-b785-70df105597f7"}
Substitute the values in green with your list name and the ID of the view


For more examples see SharePoint Online SPOMod: Get-SPOListView

PnP
SharePoint Patterns and Practices (PnP) contains a library of PowerShell commands (PnP PowerShell) that allows you to perform complex provisioning and artifact management actions towards SharePoint. The commands use CSOM and can work against both SharePoint Online as SharePoint On-Premises.

Connect to SharePoint site:

Copy
Connect-PnPOnline
Get view by its title:

Copy
Get-PnPView -List "Demo List" -Identity "Demo View"
OR
by its ID:


Copy
Get-PnPView -List "Demo List" -Identity "5275148a-6c6c-43d8-999a-d2186989a661"
For more examples see Get-PnPView website at Microsoft Docs.
