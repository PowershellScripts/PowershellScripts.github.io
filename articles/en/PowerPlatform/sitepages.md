---
layout: page
title: 'Site Pages library in Power Automate Flow'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-01-05'
---

This article shows how to use Site Pages library in Power Automate flows.

<h1> Problem </h1>

If you are trying to use Site Pages Library for actions such as Get File or Get File Properties, it will not come up in the suggestions in your dropdown

<img src="/articles/img/PASitePges.png" >

If you type the Site Pages library name manually, you may receive the following error when saving your Power Automate flow:


Fehler beim Speichern des Flows. Code: DynamicOperationRequestClientFailure, Meldung: "The dynamic operation request to API 'sharepointonline' operation 'GetTable' failed with status code 'NotFound'. This may indicate invalid input parameters. Error response: { "status": 404, "message": "List not found\r\nclientRequestId: 9cc950f7-8c3c-464c-aeda-4ab150fe9631\r\nserviceRequestId: 59eb74a1-9096-b000-1d13-482b44bc7159" }".

<img src="/articles/img/PASitePges2.png" >


<h1> Solution </h1>

In your Power Automate flow use Library GUID instead of the Site Pages Library name to solve it.

<img src="/articles/img/PASitePges3.png" >


<br/><br/>

## How to find Library GUID?

Go to Site Pages Library >> Gear Icon >> Library Settings  >> Copy the GUID from url


<img src="/articles/img/PASitePges4.png" >


Make sure you remove the HTML encoding %7B and %7D.


A short video showing how to get the Library GUID 

<video src="/articles/vid/LibraryGUID.mp4"  controls></video>