---
layout: page
title: 'Recycle vs Delete SharePoint Online list'
menubar: docs_menu
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2022-06-01'
---

## Introduction
Using SharePoint HTTP connector called ```Send an HTTP request to SharePoint in Power Automate``` gives you freedom to use REST API and perform multiple actions that have not been predefined in Power Automate actions.


## Delete vs Recycle 
So what's the difference? The difference is in Recycle Bin. If you use DELETE Method, the list will be hard-deleted and no longer recoverable from a recycle bin. If you want your list to appear in a recycle bin after you have deleted it, you should use POST Method and recycle() endpoint.  


## Delete SharePoint List
If you want to hard delete your SharePoint list, use _api/web/lists/getbytitle('YOURLISTNAME') endpoint and DELETE method.

 <br/>
<img src="/articles/images/recycleVSdelete2.png" width="200">
<br/>



## Recycle SharePoint List
If you want to recycle your list and find it later in the recycle bin of your site, use _api/web/lists/getbytitle('YOURLISTNAME')/recycle() endpoint and POST method.
 
<br/>
<img src="/articles/images/recycleVSdelete.PNG" width="200">
<br/>


## See Also

[Avoid concurrency using Etag in SharePoint REST-API Call Jump](https://www.codesharepoint.com/sharepoint-tutorial/avoid-concurrency-using-etag-in-sharepoint-rest-api-call)

<!-- Default Statcounter code for Recycle vs Delete SPO list
https://powershellscripts.github.io/articles/English/PowerPlatform/Recycle%20vs%20Delete%20SharePoin
-->
<script type="text/javascript">
var sc_project=12763875; 
var sc_invisible=0; 
var sc_security="1aaa3cd5"; 
var scJsHost = "https://";
document.write("<sc"+"ript type='text/javascript' src='" +
scJsHost+
"statcounter.com/counter/counter.js'></"+"script>");
</script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/12763875/0/1aaa3cd5/0/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
