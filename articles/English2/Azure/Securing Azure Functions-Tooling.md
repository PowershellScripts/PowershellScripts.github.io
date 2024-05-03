---
layout: page
title: 'Securing Azure Functions: Tooling'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-01-27'
---

There are several great tools that will help you troubleshoot and test your scenario.

<h1>Postman</h1>

All API calls can be tested using Postman. You can [download Postman software](https://www.postman.com/downloads/) for free from the [official Postman site](https://www.postman.com/). If installing software is not possible, due to Proxy issues, Company policies, or other restrictions, there is an [online version of Postman](https://blog.postman.com/announcing-postman-for-the-web-now-in-open-beta/). You sign up and it works beautifully. I highly recommend it. I did get an occasional [CORS issue](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/Errors) when my network setup was really muddy, but 99% of the time it's easy to use and also very portable - which plays an important role if you often switch between machines and environments.


Postman offers an option to save your credentials and speed up your calls, but since authentication is exactly the thing you will be testing - do not use that option.

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc15.png" width="400">




Sending the request for the JWT requires the following:
* POST
* client_id
* client_secret
* grant_type
* scope

 
 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc16.png" width="400">

Set **Grant_type** to "**client_credentials**". Make sure the scope is the same scope you defined inside your app registration:

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc17.png" width="200">

 <br/>
 
 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc18.png" width="600">







<h1>Application Insights</h1>

Application Insights is an extension of Azure Monitor and provides Application Performance Monitoring (also known as “APM”) features. It is well integrated with Azure Functions with no additional coding effort. To start using Application Insights, navigate to your Function App, scroll down, and click **Turn on Application Insights**

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc19.png" width="400">


Make sure the Application Insights are also gathering logs for API Management:

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc20.png" width="400">
 
Verify your Application Insights setup by sending e.g. an expired token. You should see an Exception like this in the logs:

   <img src="/articles/images/SecureAzFunc/Github-SecAzFunc21.png" width="400">




<h3>Selected features</h3>

Some of the cool features of Application Insights are:
* OOTB Availability Tests that monitor if your app is up and running

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc22.png" width="400">

* OOTB Alerts sent by email or SMS

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc23.png" width="400">

* Ability to see an overview of general app performance and to drill down into properties of each exception (you can customize these too)

   <img src="/articles/images/SecureAzFunc/Github-SecAzFunc24.png" width="400">

* Ability to group results in order to identify patterns and recurring issues

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc25.png" width="400">







<h1>Trace</h1>
Trace is one of the test options within the API Management. It allows you to quickly test your calls. Mind you, trace logs may contain sensitive information such as keys, access tokens, passwords, internal hostnames, and IP addresses. Be careful when sharing trace logs from API Management.

<h3>How to enable it?</h3>

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc26.png" width="400">
 



<h1>Monitor executions with History</h1>
Every Azure Function needs a storage, where you will find 2 tables:

* History
* Instances

The data contains the output and input of the Azure Function, as well as the times when the Azure Function was called.

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc27.png" width="400">
 



<h1>See Also</h1>

[Securing Azure Functions: Design Considerations](https://powershellscripts.github.io/articles/en/Azure/Securing%20Azure%20Functions%20-%20Design/)

[Securing Azure Functions: Scope](https://powershellscripts.github.io/articles/en/Azure/Securing%20Azure%20Functions%20-%20Scope/)

[Securing Azure Functions: API Management Policies](https://powershellscripts.github.io/articles/en/Azure/Securing%20Azure%20Functions%20-%20API/)


<!-- Default Statcounter code for Azure - Tooling
https://powershellscripts.github.io/articles/en/Azure/Securing%20Azure%20Functions-Tooling/
-->
<script type="text/javascript">
var sc_project=12962358; 
var sc_invisible=0; 
var sc_security="519da304"; 
var scJsHost = "https://";
document.write("<sc"+"ript type='text/javascript' src='" +
scJsHost+
"statcounter.com/counter/counter.js'></"+"script>");
</script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/12962358/0/519da304/0/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->

