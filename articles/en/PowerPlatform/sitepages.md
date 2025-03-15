---
layout: page
title: 'Site Pages library in Power Automate Flow'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-01-05'
---

This article shows how to use Site Pages library in Power Automate flows.

<h1> Problem </h1>

If you are trying to use Site Pages library for Power Automate triggers and actions such as **Get File**, **When a file is deleted** or **When a file is created or modified (properties only)**, the Site Pages library will not come up in the suggestions in your dropdown

<img src="/articles/img/PASitePages.png" width="600" alt="power automate screenshot" >


If you type the Site Pages library name manually, you may receive the following error when saving your Power Automate flow:

<span style="color:red">
Fehler beim Speichern des Flows. Code: DynamicOperationRequestClientFailure, Meldung: "The dynamic operation request to API 'sharepointonline' operation 'GetTable' failed with status code 'NotFound'. This may indicate invalid input parameters. Error response: { "status": 404, "message": "List not found\r\nclientRequestId: 9cc950f7-8c3c-464c-aeda-4ab150fe9631\r\nserviceRequestId: 59eb74a1-9096-b000-1d13-482b44bc7159" }".

</span>

<img src="/articles/img/PASitePages2.png" width="600" alt="power automate screenshot" >


<h1> Solution </h1>

In your Power Automate flow use Library GUID instead of the Site Pages Library name to solve it.

<img src="/articles/img/PASitePages3.png" width="600" alt="power automate screenshot" >


<br/><br/>

## How to find Library GUID?

Go to Site Pages Library >> Gear Icon >> Library Settings  >> Copy the GUID from url


<img src="/articles/img/PASitePages4.png" width="600" alt="library GUID" >


Make sure you remove the HTML encoding %7B and %7D.


<br/><br/>

#### Video Guide how to get Library GUID

A short video showing how to get the Library GUID 

<iframe width="660" height="400" src="https://www.youtube.com/embed/rb2-SkWlN44" frameborder="0" allowfullscreen></iframe>






<!-- Default Statcounter code for PP Site Pages
https://powershellscripts.github.io/articles/en/PowerPlatform/sitepages/
-->
<script type="text/javascript">
var sc_project=13075445; 
var sc_invisible=1; 
var sc_security="25adc6d8"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="hit counter"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13075445/0/25adc6d8/1/"
alt="hit counter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->