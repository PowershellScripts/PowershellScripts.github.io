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



<br/><br/>

<h1>Authorization Flow</h1>

Make sure you understand [authorization flow](https://learn.microsoft.com/en-us/azure/api-management/authorizations-overview#process-flow-for-runtime). 

<img src="/articles/images/SecureAzFunc/Github-secureayfunc2.svg" width="600" alt="Diagram that shows the process flow for creating runtime.">


The client app needs to call API Management. If you can call your Azure Function directly, using only function code - it's not secured with OAuth.

<br/><br/>
<h1>JSON Web Token (JWT)</h1>

Per the OAuth specification, access tokens are opaque strings without a set format. 

JSON Web Tokens are split into three parts:
* Header - Provides information about how to validate the token including information about the type of token and how it was signed.
* Payload - Contains all of the important data about the user or application that's attempting to call the service.
* Signature - Is the raw material used to validate the token. Each piece is separated by a period (.) and separately Base64 encoded.

Useful resources:
* [List of token claims and their description](https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens#claims-in-access-tokens)
* [Options for validating a token](https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens#validate-tokens)

<h3>Token formats</h3>

There are two different versions of JSON Web Tokens (JWTs) called v1.0 and v2.0 available on the Microsoft identity platform. Custom APIs registered by developers in Azure Active Directory can choose either of them. Microsoft-developed APIs like Microsoft Graph or APIs in Azure use other, proprietary token formats.

v1.0 is selected as a default for Azure AD-only applications. 

v2.0 is selected as a default for applications that support consumer accounts. 

The contents of the token are not decoded directly in applications and are intended for the API only, however, for troubleshooting purposes you can decode your tokens using [https://jwt.ms/](https://jwt.ms/)  or [https://jwt.io/](https://jwt.ms/)https://jwt.ms/  sites.


V1.0 

Decoded v1.0 token example (the token value comes from [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens) ):

 <img src="/articles/images/SecureAzFunc/Github-SecureAzFunc3.PNG" width="600">



V2.0

Decoded v2.0 token example (the token value comes from [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens) ):
 
<img src="/articles/images/SecureAzFunc/Github-SecAzFunc4.png" width="600">


The site [https://jwt.ms/](https://jwt.ms/)  also helps to interpret the claims included in the token:

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc5.png" width="400">


    ! Notice the difference in the issuers between the two versions. 
    Token version is one of the common issues while configuring authentication between apps!


<h3>What does it mean that the token version is wrong?</h3>

* If there is a mismatch between issuers, i.e. in the decoded token you see sts.windows.net and APIM policy requires login.microsoftonline.com 
* When you receive "Invalid token" error


<h3>How to "fix" token version?</h3>

Go to manifest in your app registration and set accessTokenAcceptedVersion
to 2:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc6.png" width="400">

 

It may take a few minutes to apply the changes.
