---
layout: page
title: 'Manage Viva Engage with Graph API'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-06-02'
---


<h1>Graph Explorer</h1>

[Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) is a great tool for showcasing the capabilities of the Graph API. It even comes with a sample tenant you can test your queries against. The same queries can, of course, be reproduced in Postmant and your future applications. For our purpose of testing the Viva Engage Community API, we will use Graph Explorer. If it's your first time using Graph Explorer or Graph API, you can check out the [Graph Explorer Overview](https://learn.microsoft.com/en-us/graph/graph-explorer/graph-explorer-overview).

<br/><br/>

<h1>Get all communities</h1>

In order to get all communities using Graph API, use the GET method with the following url: https://graph.microsoft.com/beta/employeeexperience/communities

<img src="/articles/images/VivaGraphAPI.PNG">

<br/><br/>

<h1>Get specific community</h1>

In order to get a specific community, use its ID. In the case of the sample tenant used in Graph Explorer, it's eyJfdHlwZSI6Ikdyb3VwIiwiaWQiOiIxMjczMDI1MyJ9. Use the GET method with the following url: https://graph.microsoft.com/beta/employeeexperience/communities/eyJfdHlwZSI6Ikdyb3VwIiwiaWQiOiIxMjczMDI1MyJ9

<img src="/articles/images/VivaGraphAPI2.PNG">

See the full documentation for this GET request [here](https://learn.microsoft.com/en-us/graph/api/community-get?view=graph-rest-beta&tabs=http).

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

Every Viva Engage community is associated with a Microsoft 365 group, but the group doesn't have the same ID as the community. 

<img src="/articles/images/VivaGraphAPI3.PNG">


<br/><br/>

<h1>Modify your community</h1>

For Viva Engage networks in native mode, creating a new Viva Engage community also generates a connected Microsoft 365 group, as well as a new SharePoint site, OneNote notebook, and Planner plan. You can use the associated Microsoft 365 group to manage community operations, such as:

* Add or remove group members
* Manage group ownership
* Delete a group
* Rename a group
* Update the group description


<h1>See Also</h1>

[Introducing the Community Creation API for Viva Engage on Microsoft Graph Beta ](https://techcommunity.microsoft.com/t5/viva-engage-blog/introducing-the-community-creation-api-for-viva-engage-on/ba-p/4011966
)
