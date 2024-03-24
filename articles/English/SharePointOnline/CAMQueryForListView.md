---
layout: page
title: 'Easy way to create CAML Query for list view'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2023-12-30'
---


<h1>CamlQuery</h1>
When loading thousands of SharePoint list items, you may want to limit the number of retrieved results. If your list contains 10 elements, then loading them like this will present no issues. 

```
CamlQuery camlQuery = new CamlQuery();
camlQuery.ViewXml = "<View Scope=\"RecursiveAll\"></View>";
ListItemCollection collListItem = list.GetItems(camlQuery);
clientContext.Load(collListItem);
clientContext.ExecuteQuery();
```

Loading thousands of items in such manner is highly inefficient, if you need only few items that fulfill specific requirement. The ```GetItems(CamlQuery)``` method allows you to define a Collaborative Application Markup Language (CAML) query that specifies which items to return. You can pass an undefined CamlQuery object to return all items from the list, as in the example above, or use the ViewXml property to define a CAML query and return items that meet specific criteria.
<br/><br/>
The Where clause translates into the SQL SELECT statement with a mixture of logical joins (\<And>, \<Or>), comparison operators, such as \<Contains>, \<Geq>, \<Lt>, simple arithmetic operators, field (column) references, constant values, and predefined Collaborative Application Markup Language (CAML) constants. 


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

<h1>How</h1>

<h2>Step 1: Create in GUI</h2>

While a simple query is often more easily typed directly, writing CamlQuery in a complex scenario may prove to be time-consuming and prone to error. A way around it is to use GUI and create a specific view in SharePoint list/library:

                     

You can set the view to be private so that your users do not see this temporary view.

<h2>Step 2: Extract the CamlQuery</h2>
Now that the view is created and you see in the list that only items fulfilling your criteria are displayed, proceed to extract the ViewXML.

<h3>Using REST Endpoint</h3>
Find list and view's GUID. Open the created list view in edit mode and copy the url:

https://tenant.sharepoint.com/\_layouts/15/ViewEdit.aspx?**List=%7B4E998310-BCE0-4647-BB77-0183EA3E48A8%7D**&**View=%7B47FA121F-B26F-4CCD-B785-70DF105597F7%7D**&Source=%252Fsites%252FtestFlow%252F%255Flayouts%252F15%252Flistedit%252Easpx%253FList%253D%25257B4e998310%252Dbce0%252D4647%252Dbb77%252D0183ea3e48a8%25257D

List GUID is after List= , and marked in green in the example above . %7B is url encoded opening parentheses { and %7D is URL-encoded closing parentheses** } . **View GUID is after View= and marked in orange in the example above.

In your browser enter 


https://TENANT.sharepoint.com/sites/SITENAME/_api/web/lists/getbyid('4E998310-BCE0-4647-BB77-0183EA3E48A8')/Views/getbyid('47FA121F-B26F-4CCD-B785-70DF105597F7')
replacing your tenant and site collection names, the list GUID and view GUID with values from the view created in the previous point. I like to use Internet Explorer for this step, because it renders a very nice and readable XML:


Take the value from ViewQuery. 
In this example it is:

```powershell
<OrderBy><FieldRef Name="Modified" Ascending="FALSE" /></OrderBy><Where><Eq><FieldRef Name="Editor" /><Value Type="User">FR</Value></Eq></Where>
```

<h3>Using Powershell CSOM</h3>

Load SharePoint Online SDKs. The current version at the time of writing this article and used in the example is available at  NuGet gallery.

```powershell
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"
```

Create SharePoint context and assign credentials of a user who has enough access to the list and its views. Next load the list, the view and its ViewFields. If you do not load the ViewFields, you will still be able to access the view properties, but not output the entire $view object.

```powershell
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
$ll=$ctx.Web.Lists.GetByTitle($ListTitle)
$ctx.load($ll)
$ctx.load($ll.Views)
$ctx.ExecuteQuery()

$view = $ll.Views.GetById($ViewGUID) # If you want to use view's name instead of its GUID, use .GetByTitle method, like this: $view = $ll.Views.GetByTitle($ViewTitle)
$ctx.Load($view)
$ctx.Load($view.ViewFields)
$ctx.ExecuteQuery()
```

Display the view and its properties:


Write-Output $view
or export it to CSV using:

Export-CSV $view
 


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
