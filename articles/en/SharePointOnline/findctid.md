---
layout: page
title: 'What is SharePoint Online content type ID?'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-11-23'
---

# What is SharePoint Online Content Type ID?

Content type IDs in SharePoint uniquely identify the content type, such as Item, Document, Announcement. An example of a content type ID is 0x01 which is an Item Content Type ID. Content types based on other content types are often called child content types.

They are designed to be recursive. The content type ID shows that content type's **lineage**, or the line of parent content types from which the content type inherits. Each SharePoint content type ID contains the ID of the parent content type, which in turn contains the ID of that content type's parent, and so on, ultimately back to and including the System content type ID. SharePoint uses this information to [determine the relationship between content types](https://learn.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/aa543822(v=office.14)?redirectedfrom=MSDN) and for push-down operations.

You can construct a valid SharePoint content type ID using one of two conventions:

* Parent content type ID + two hexadecimal values (the two hexadecimal values cannot be "00")
* Parent content type ID + "00" + hexadecimal GUID

 <img src="/articles/images/Github-AddContentType2-1.png" width="400"><br/>
<sup>Source: https://msdn.microsoft.com/en-us/library/office/aa543822%28v=office.14%29.aspx?f=255&MSPPError=-2147217396</sup>



An ID of a custom content type based on Item, will look like this when using GUID approach: <br/>
0x01   - which is an Item Content Type ID<br/>
00     - a kinf of "space" to differentiate where the parent ends and where the child content type begins<br/>
9e862727eed04408b2599b25356e7914  - the hexadecimal value <br/>


Item Content Type ID	following the convention	 the hexadecimal value 
0x01	00   	9e862727eed04408b2599b25356e7914
Altogether: 0x01009e862727eed04408b2599b25356e7914
 
You can create a guid automatically using Windows Powershell 

```powershell
[guid]::NewGuid()
```


You can also invent your own content type ID :)  Using 0x0100aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa works :)  though it's not exactly the best practice.

Each SharePoint content type ID must be **unique within a SharePoint site collection**. If the code you are writing does not specify the guid, SharePoint will assign one to the content type, based on the parent content type.


<br/><br/>

<h1>How to find SharePoint Online content type ID using UI</h1>



Navigate to list settings, under Advanced enable Content Type Management and read the content type ID from the URL:

<video src="/articles/vid/ctid.mp4"  controls></video>


<br/><br/>


<h1>Using REST API and Browser</h1>

There is a nice trick how you can use GET Rest API directly in the browser without creating scripts or applications.  

Open your browser, log in to your tenant and enter the following URL:

 https://**TENANT**.sharepoint.com/sites/**SITENAME**/_api/web/lists/getbytitle('**LISTNAME**')/contenttypes

e.g.
https://acco967.sharepoint.com/sites/EWZ/_api/web/lists/getbytitle('test1')/contenttypes


<video src="/articles/vid/ctid2.mp4"  controls></video>


<br/>

You will get a lovely XML of all your content types  (more lovely in Edge and Firefox than Chrome)

 <img src="/articles/img/ctid22.PNG" width="400"><br/>


  <img src="/articles/img/ctid23.PNG" width="400"><br/>






<br/><br/>

# See Also

[Find content type ID using Powershell](https://powershellscripts.github.io/articles/en/SharePointOnline/findctIDPS/)



<!-- Default Statcounter code for findctid
https://powershellscripts.github.io/articles/en/SharePointOnline/findctid/
-->
<script type="text/javascript">
var sc_project=13065137; 
var sc_invisible=1; 
var sc_security="a877695d"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13065137/0/a877695d/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->