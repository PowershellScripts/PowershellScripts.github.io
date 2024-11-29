---
layout: page
title: 'Easy way to create CAML Query for list view'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-03-24'
---

<h1>Easy CamlQuery</h1>
When loading thousands of SharePoint list items, you may want to limit the number of retrieved results. If your SharePoint list contains 10 elements, then loading them like this will present no issues. 

```
CamlQuery camlQuery = new CamlQuery();
camlQuery.ViewXml = "<View Scope=\"RecursiveAll\"></View>";
ListItemCollection collListItem = list.GetItems(camlQuery);
clientContext.Load(collListItem);
clientContext.ExecuteQuery();
```

Loading thousands of SharePoint items in such manner is highly inefficient, if you need only a few items that fulfill specific requirement. The ```GetItems(CamlQuery)``` method allows you to define a Collaborative Application Markup Language (CAML) query that specifies which items to return. You can pass an undefined CamlQuery object to return all items from the list, as in the example above, or use the ViewXml property to define a CAML query and return items that meet specific criteria.
<br/><br/>
The Where clause translates into the SQL SELECT statement with a mixture of logical joins (\<And>, \<Or>), comparison operators, such as \<Contains>, \<Geq>, \<Lt>, simple arithmetic operators, field (column) references, constant values, and predefined Collaborative Application Markup Language (CAML) constants. 
<br/>
Creating a complex query, however, can present quite a challenge. To make sure every slash and value are exactly where they should be, it's easier to create a view using User Interface and then copy the CAML Query behind it. 

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

<br/><br/><br/>


<h1>How</h1>
<br/>
<h2>Step 1: Create in GUI</h2>

A simple query is often more easily typed directly. Writing CamlQuery in a complex scenario is usually time-consuming and prone to error. A way around it is to use GUI and create a specific view in SharePoint list/library:

<img src="/articles/images/easycaml1.png" width="200"><br/>

 <img src="/articles/images/easycaml2.png"><br/>

You can set the view to be private so that your users do not see this temporary view.

<h2>Step 2: Extract the CamlQuery</h2>
Now that the view is created and you verified you see only the selected items, proceed to extract the ViewXML.

<h3>Method 1: Using REST Endpoint and browser</h3>
Find list and view's GUID. Open the created list view in edit mode and copy the url:

 <img src="/articles/images/easycaml3.png"><br/>
 
https://tenant.sharepoint.com/\_layouts/15/ViewEdit.aspx?**List=%7B4E998310-BCE0-4647-BB77-0183EA3E48A8%7D**&**View=%7B47FA121F-B26F-4CCD-B785-70DF105597F7%7D**&Source=%252Fsites%252FtestFlow%252F%255Flayouts%252F15%252Flistedit%252Easpx%253FList%253D%25257B4e998310%252Dbce0%252D4647%252Dbb77%252D0183ea3e48a8%25257D

* List GUID is after List= , and in the example above the value is <b>4E998310-BCE0-4647-BB77-0183EA3E48A8</b>. 

* %7B is url encoded opening parentheses { and %7D is URL-encoded closing parentheses** } . 

* View GUID is after View= and in the example above has the value of <b>47FA121F-B26F-4CCD-B785-70DF105597F7</b>.

In your browser enter 
```
https://TENANT.sharepoint.com/sites/SITENAME/_api/web/lists/getbyid('4E998310-BCE0-4647-BB77-0183EA3E48A8')/Views/getbyid('47FA121F-B26F-4CCD-B785-70DF105597F7')
```
replacing your tenant and site collection names, the list GUID and view GUID with values from the view created in the previous point. I like to use Internet Explorer for this step, because it renders a very nice and readable XML:

 <img src="/articles/images/easycaml4.png"><br/>

 
Take the value from ViewQuery. 
In this example it is:

```powershell
<OrderBy><FieldRef Name="Modified" Ascending="FALSE" /></OrderBy><Where><Eq><FieldRef Name="Editor" /><Value Type="User">FR</Value></Eq></Where>
```


<br/><br/><br/>


<h3>Method 2: Using Powershell CSOM</h3>

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

```powershell
Write-Output $view
```

or export it to CSV using:

```powershell
Export-CSV $view
```

 
 <img src="/articles/images/easycaml5.png"><br/>

<br/><br/><br/>


<h3>Method 3: PnP Powershell</h3>
SharePoint Patterns and Practices (PnP) contains a library of PowerShell commands (PnP PowerShell) that allows you to perform complex provisioning and artifact management actions towards SharePoint. The commands use CSOM and can work against both SharePoint Online as SharePoint On-Premises.

Connect to SharePoint site:

```powershell
Connect-PnPOnline
```

Get view by its title:

```powershell
Get-PnPView -List "Demo List" -Identity "Demo View"
```

OR
by its ID:


```powershell
Get-PnPView -List "Demo List" -Identity "5275148a-6c6c-43d8-999a-d2186989a661"
```

For more examples see [Get-PnPView](https://pnp.github.io/powershell/cmdlets/Get-PnPView.html) cmdlet description.


<!-- Default Statcounter code for CamlQueryforListView
https://powershellscripts.github.io/articles/en/SharePointOnline/CAMLQueryForListView
-->
<script type="text/javascript">
var sc_project=12981483; 
var sc_invisible=1; 
var sc_security="fb5df1d1"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/12981483/0/fb5df1d1/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
