---
layout: page
title: 'Securing Azure Functions: Design Considerations'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-01-25'
---



<h1>Design</h1>

Which app calls which? One of the first things to consider is the [design](https://learn.microsoft.com/en-us/azure/api-management/authentication-authorization-overview#oauth-20-authorization-scenarios) you want to follow. The authentication flow you choose will decide what is authorised to what. Consider these two scenarios:

<img src="/articles/images/SecureAzFunc/Github-SecureAzFunc1.PNG" width="600"  alt="Diagram showing OAuth communication where audience is the backend.Diagram showing OAuth communication where audience is the API Management gateway.">



</br></br>
<h1>Authorization Flow</h1>

Make sure you understand [authorization flow](https://learn.microsoft.com/en-us/azure/api-management/authorizations-overview#process-flow-for-runtime). 

<img src="/articles/images/SecureAzFunc/Github-secureayfunc2.svg" width="600" alt="Diagram that shows the process flow for creating runtime.">


The client app needs to call API Management. If you can call your Azure Function directly, using only function code - it's not secured with OAuth.

