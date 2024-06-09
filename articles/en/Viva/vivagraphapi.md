---
layout: page
title: 'Manage Viva Engage with Graph API'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-06-02'
---


<h1>Graph Explorer</h1>

[Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) is a great tool for showcasing the capabilities of the Graph API. It even comes with a sample tenant you can test your queries against. The same queries can, of course, be reproduced in Postman and your future applications. For our purpose of testing the Viva Engage Community API, we will use Graph Explorer. If it's your first time using Graph Explorer or Graph API, you can check out the [Graph Explorer Overview](https://learn.microsoft.com/en-us/graph/graph-explorer/graph-explorer-overview).

<br/><br/>

<h1>Get all communities</h1>


| Method |	Url |
|---|---|
|GET| https://graph.microsoft.com/beta/employeeexperience/communities |

In order to get all Viva Engage communities using Graph API, use the GET method with the following url: https://graph.microsoft.com/beta/employeeexperience/communities

<img src="/articles/images/VivaGraphAPI.PNG">

<br/><br/>

<h1>Get specific community</h1>

<h3> a) by id </h3>


| Method |	Url |
|---|---|
|GET| https://graph.microsoft.com/beta/employeeexperience/communities/COMMUNITYID |


In order to get a specific Viva Engage community, use its ID. In the case of the sample tenant used in Graph Explorer, it's eyJfdHlwZSI6Ikdyb3VwIiwiaWQiOiIxMjczMDI1MyJ9. Use the GET method with the following url: https://graph.microsoft.com/beta/employeeexperience/communities/eyJfdHlwZSI6Ikdyb3VwIiwiaWQiOiIxMjczMDI1MyJ9

<img src="/articles/images/VivaGraphAPI2.PNG">


<h3> b) by name </h3>

| Method |	Url |
|---|---|
|GET| https://graph.microsoft.com/beta/employeeexperience/communities?$filter=displayName eq 'Marketing' |


Use **$filter** to filter for your Viva Engage community's name. The API call fetches details about a community named "Marketing" and returns data such as community ID, display name, description, and other related properties.

<img src="/articles/images/VivaGraphAPI5.PNG">



See the full documentation for this GET request [here](https://learn.microsoft.com/en-us/graph/api/community-get?view=graph-rest-beta&tabs=http).

<br/><br/>

<h1>Get community owners</h1>

| Method |	Url |
|---|---|
|GET| https://graph.microsoft.com/beta/employeeexperience/communities/COMMUNITYID/owners |



Using GET Method and Url https://graph.microsoft.com/beta/employeeexperience/communities/COMMUNITYID/owners  you can also retrieve the owners of the community.

<img src="/articles/images/VivaGraphAPIOwners.PNG">


Use **?$select=displayName** to select the properties of the owners that interest you:

<img src="/articles/images/VivaGraphAPIOwners2.PNG">


<br/><br/>

<h1>Get associated group</h1>

| Method |	Url |
|---|---|
|GET| https://graph.microsoft.com/beta/employeeexperience/communities/COMMUNITYID/group |



Gets the Microsoft 365 group associated with the Viva Engage community. The M365 group is crucial for the management of the community. You can manage community operations through the associated Microsoft 365 group by adding or removing members, managing ownership, deleting or renaming the group, and updating its description. 


<img src="/articles/images/VivaGraphAPIgroup.PNG">

<br/><br/>

<h1>community resource type</h1>

Represents a community in Viva Engage. Each community is linked to a Microsoft 365 group, although the group does not share the same ID as the community.

| Property |	Type	| Description |
|---|---|---|
description	| String	| The description of the community. The maximum length is 1024 characters.
displayName	| String	| The name of the community. The maximum length is 255 characters.
groupId	| String	| The ID of the Microsoft 365 group that manages the membership of this community.
id	| String	| The unique identifier of the community. Read only. Inherited from entity.
privacy	| communityPrivacy	| Defines the privacy level of the community. The possible values are: public, private, unknownFutureValue (don't use).


<br/><br/>

<h1>Create a new community</h1>

| Method |	Url |
|---|---|
|POST| https://graph.microsoft.com/beta/employeeexperience/communities/COMMUNITYID |

Mind that every Viva Engage community is associated with a Microsoft 365 group, but the group doesn't have the same ID as the community. 

<img src="/articles/images/VivaGraphAPIZZZ.PNG">


<br/><br/>

<h1>Modify your community</h1>

For Viva Engage networks in native mode, creating a new Viva Engage community also generates a connected Microsoft 365 group, as well as a new SharePoint site, OneNote notebook, and Planner plan. You can use the associated Microsoft 365 group to manage community operations, such as:

* Add or remove group members
* Manage group ownership
* Delete a group
* Rename a group
* Update the group description


<br/><br/>

<h1>Summary</h1>


| Method |	Url | Description |
|---|---|---|
|GET| https://graph.microsoft.com/beta/employeeexperience/communities | Gets all communities |
|GET| https://graph.microsoft.com/beta/employeeexperience/communities/COMMUNITYID | Gets specific community|
|GET| https://graph.microsoft.com/beta/employeeexperience/communities?$filter=displayName eq 'Marketing' | Gets community by name |
|GET| https://graph.microsoft.com/beta/employeeexperience/communities/COMMUNITYID/owners | Gets community owners |
|GET| https://graph.microsoft.com/beta/employeeexperience/communities/COMMUNITYID/group | Gets associated M365 group |
|POST| https://graph.microsoft.com/beta/employeeexperience/communities/COMMUNITYID | Creates a community|


<br/><br/>

<h1>See Also</h1>

[Introducing the Community Creation API for Viva Engage on Microsoft Graph Beta ](https://techcommunity.microsoft.com/t5/viva-engage-blog/introducing-the-community-creation-api-for-viva-engage-on/ba-p/4011966
)





<!-- Default Statcounter code for Viva Graph API
https://powershellscripts.github.io/articles/en/Viva/vivagraphapi/
-->
<script type="text/javascript">
var sc_project=13006360; 
var sc_invisible=0; 
var sc_security="8e4900b1"; 
var scJsHost = "https://";
document.write("<sc"+"ript type='text/javascript' src='" +
scJsHost+
"statcounter.com/counter/counter.js'></"+"script>");
</script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13006360/0/8e4900b1/0/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
