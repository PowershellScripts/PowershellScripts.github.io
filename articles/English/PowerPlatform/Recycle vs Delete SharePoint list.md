## Introduction
Using SharePoint HTTP connector called Send an HTTP request to SharePoint in Power Automate gives you freedom to use REST API and perform multiple actions that have not been predefined in Power Automate actions.


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