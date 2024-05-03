---
layout: page
title: 'Secure Azure Functions with API Management'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2023-12-15'
---

<sup>Disclaimer 
I am not an Azure expert. I haven't been doing it for the last 50 years and I haven't tested every nook and cranny of the service. I just did a considerable number of Azure Functions with API Management, and troubleshooted all kinds of errors. Together it amounted to quite a compendium of testing/verification/troubleshooting steps that I wanted to share with you and the future me  :)
</sup>


<h1>Design</h1>

Which app calls which? One of the first things to consider when planning your API Management (called APIM in short) is the [design](https://learn.microsoft.com/en-us/azure/api-management/authentication-authorization-overview#oauth-20-authorization-scenarios) you want to follow. The authentication flow you choose will decide what is authorised to what. Consider these two scenarios:

<img src="/articles/images/SecureAzFunc/Github-SecureAzFunc1.PNG" width="600"  alt="Diagram showing OAuth communication where audience is the backend.Diagram showing OAuth communication where audience is the API Management gateway.">

<sup>
Image source:  https://learn.microsoft.com/en-us/azure/api-management/authentication-authorization-overview
</sup>

The first scenario is the most common. Azure API Management is a "transparent" proxy between the caller and backend API. This is what most tutorials, guides and troubleshooters refer to. You can still configure policies in APIM to validate the token, and check other claims of interest extracted from the token, but the calling application requests access to the API directly. The scope of the access token is between the **calling application and backend API**. 

In the second scenario, the API Management service acts on behalf of the API. The scope of the access token is between the **calling application and API Management**. The second scenario will be usually used when calling the backend API is not possible. For example, when the [backend API does not support OAuth](https://learn.microsoft.com/en-us/azure/api-management/authentication-authorization-overview#audience-is-api-management).


<br/><br/>

<h1>Authorization Flow</h1>

Make sure you understand [authorization flow](https://learn.microsoft.com/en-us/azure/api-management/authorizations-overview#process-flow-for-runtime) when setting up your Azure API Management. 

<img src="/articles/images/SecureAzFunc/Github-secureayfunc2.svg" width="600" alt="Diagram that shows the process flow for creating runtime.">


The client app needs to call API Management. Test it. If you can call your Azure Function directly, using only the function code - it's not secured with OAuth.



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

The contents of the token are not decoded directly in applications and are intended for the API only, however, for troubleshooting purposes you can decode your JSON Web Tokens using [https://jwt.ms/](https://jwt.ms/)  or [https://jwt.io/](https://jwt.io/)  sites.


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

* If there is a mismatch between issuers, i.e. in the decoded token you see **sts.windows.net** and API Management policy requires **login.microsoftonline.com** 
* When you receive "Invalid token" error

<h3>How to "fix" token version?</h3>

Go to Azure Portal, to your app registration. Inside the Azure app registration, open manifest and set accessTokenAcceptedVersion to 2:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc6.png" width="400">

 

It may take a few minutes to apply the changes. Get a coffee :)

<br/><br/>

<h1>Scope</h1>
When setting scope for your application permissions, mind the different access scenarios:

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc7.png" width="400">
<sup>Image source: https://learn.microsoft.com/en-us/azure/active-directory/develop/permissions-consent-overview</sup>

For more details on delegated vs app-only access see [Permissions and consent](https://learn.microsoft.com/en-us/azure/active-directory/develop/permissions-consent-overview).

To simplify it though, let us say that if you need a user to click consent to a particular resource, then you should go here in the Azure portal:

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc8.png" width="500">


But in the client app and Backend API scenario describe above, you would rather use app-only permissions, and start from Enterprise Applications, where you set  **Assignment Required** to **True**.

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc9.png" width="500">


If this option is set to yes, then users and other apps or services must first be assigned this application before being able to access it.

<h3>Why is it important?</h3>
Because if you do not enable required assignment, **any app** registration can request the default scope of your app. This is the default setting. **Unless you are doing further checking of the token scopes in code, this leaves your app open** to being called by clients which haven’t been explicitly granted permission.  Check out [these cool tests](https://medium.com/airwalk/azure-app-service-easy-auth-and-the-default-scope-1fb0b65b4d26) proving how that happens.


<h3>Application is not assigned to a role for the application</h3>

Once you have enabled the required assignment setting, but before you assigned any roles, you should get an *invalid_grant* error., e.g.

    AADSTS501051: Application '1df771d6-ef9c-473e-af87-cafa8928e024'(testClient) 
    is not assigned to a role for the application '8d138478-d3a3-47f5-a233-1408cd6baae4'(testBackend2)

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc10.png" width="600">

If you receive **Application is not assigned to a role for the application** error, go to Azure Portal >> App Registrations >> Choose your client app >> API Permissions:

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc11.png" width="400">

Add the required permission and verify that the admin consent has been granted. When you click on **Add a permission**, you can easily navigate to the right API, by selecting **my APIs** or **APIs my organization uses**:

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc12.png" width="400">

 
If you do not see the right API there, go to Azure Portal >> App Registrations >> Select your backend app (representing Azure Function) >> App roles (for app-only access scenario).


<h3>Default scope</h3>


The /.default scope has several uses:
* it is a shortcut back to the Azure AD v1 behavior (e.g., static consent)
* **it is required when the app is making service-to-service calls or using application-only permissions**
* it is required when using the on-behalf-of (OBO) flow, where your API is making calls on behalf of the user to a different API, e.g. client app --> your API --> Graph API.

  
Check out the awesome article by [John Patrick Dandison](https://dev.to/jpda) explaining in great detail [Just what *is* the /.default scope in the Microsoft identity platform & Azure AD?](https://dev.to/425show/just-what-is-the-default-scope-in-the-microsoft-identity-platform-azure-ad-2o4d) where he clarifies very well, and with examples, the difference between static and dynamic consent and where to use the ./default scope. 

For us, the second scenario (making service-to-service calls or using application-only permissions) is the most relevant. That's why our scope will look like this:
{BackendID}/.default.

For a backend app (our Azure Function) with the following data:

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc13.png" width="400">
the scope will look like this:  *8d138478-d3a3-47f5-a233-1408cd6baae4/.default*


 
<br/><br/>

<h1>API Management Policies</h1>

Azure API Management Policies allow to manage hybrid, multi-cloud APIs across all environments. As a platform-as-a-service, API Management supports the complete API lifecycle. The inbound processing rules allow you to configure a JWT validation policy to pre-authorize requests:
 
 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc14.png" width="400">


Validating JWT token is one of the many access restrictions policies that API Management allows you to configure.
Check out [API Management access restriction policies documentation](https://learn.microsoft.com/en-us/azure/api-management/api-management-access-restriction-policies) for more options.


<h3>JWT validation policy</h3>

Sample inbound policy.

    <inbound>
        <base />
        <set-backend-service id="apim-generated-policy" backend-id="provisionierungacco" />
        <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Invalid token.">
            <openid-config url="https://login.microsoftonline.com/0da700fe-a3a7-4aaa-a43f-48a79eefc326/v2.0/.well-known/openid-configuration Jump " />
            <issuers>
                <issuer>https://login.microsoftonline.com/0da700fe-a3a7-4aaa-a43f-48a79eefc326/v2.0< Jump /issuer>
            </issuers>
            <required-claims>
                <claim name="aud" match="any">
                    <value>7e5ff242-8d3a-46a9-8890-45722c2f3d27</value>
                </claim>
            </required-claims>
        </validate-jwt>
    </inbound>

             
Let us analyze the values:
* backend-id="provisionierungacco"
  
This should point to your Azure Function.

* openid-config url="https://login.microsoftonline.com/0da700fe-a3a7-4aaa-a43f-48a79eefc326/v2.0/.well-known/openid-configuration" 

Every app registration in Azure AD is provided a publicly accessible endpoint that serves its OpenID configuration document.

To find the configuration document for your app, navigate to the Azure portal and then: Select Azure Active Directory >> App registrations >> YourApp >> Endpoints. Locate the URI under OpenID Connect metadata document. It should look something like this:
https://login.microsoftonline.com/YOURTENANTID/v2.0/.well-known/openid-configuration <br/>
where 
<br/>
path is: /.well-known/openid-configuration <br/>
and authority URL is: https://login.microsoftonline.com/{tenant}/v2.0

For more information go to [OpenID Connect on the Microsoft identity platform](https://learn.microsoft.com/en-us/azure/active-directory/develop/v2-protocols-oidc).

<h3> required-claims</h3>   

<h4>aud</h4>
    
It's up to you to decide which claims will be checked. One of the more common ones is the aud which in our case will be identical with the guid in our scope.
As per[ RFC definition ](https://www.rfc-editor.org/rfc/rfc7519#section-4.1.3) aud claim refers to the recipient of the access token:
<br/>

 >   The "aud" (audience) claim identifies the recipients that the JWT is intended for. Each principal intended to process the JWT MUST identify itself with a value in the audience claim. If the principal processing the claim does not identify itself with a value in the "aud" claim when this claim is present, then the JWT MUST be rejected. In the general case, the "aud" value is an array of case- sensitive strings, each containing a StringOrURI value. In the special case when the JWT has one audience, the "aud" value MAY be a single case-sensitive string containing a StringOrURI value. The interpretation of audience values is generally application specific. Use of this claim is OPTIONAL. 

 <br/>
In our case the recipient, or the audience, will be Azure Function.
 

<h4>azp</h4>
[IANA](https://www.iana.org/assignments/jwt/jwt.xhtml)  defines azp as <i>Authorized party - the party to which the ID Token was issued</i>.

[Open ID specification](https://openid.net/specs/openid-connect-core-1_0.html) gives the following definition:
<br/>

>OPTIONAL. Authorized party - the party to which the ID Token was issued. If present, it MUST contain the OAuth 2.0 Client ID of this party. This Claim is only needed when the ID Token has a single audience value and that audience is different than the authorized party. It MAY be included even when the authorized party is the same as the sole audience. The azp value is a case sensitive string containing a StringOrURI value.

<br/>

I like to check this claim, because it helps to avoid the required assignment issue. Instead of (or additionally to) granting permissions for your client to your backend app you can check in the JWT who sends the request. If needed, several values can be accepted, e.g. in the scenario where 2 or more apps call your Azure Function:
 
        <required-claims>
            <claim name="aud" match="any">
                <value>7e5ff242-8d3a-46a9-8890-45722c2f3d27</value>
            </claim>
            <claim name="azp" match="any">
                 <value>a1888df2-84c2-4379-8d53-7091dd630ca7</value>
                 <value>f1d55d9b-b116-4f54-bc00-164a51e7e47f</value>
                 <value>d5dfkae9-4f54-bc00-8d53-164a5130ca7b</value>
            </claim>
        </required-claims>
 

 
<br/><br/>
<h1>Tooling</h1>


There are several great tools that will help you troubleshoot and test your scenario.

<h3>Postman</h3>

All API calls can be tested using Postman. You can [download Postman software](https://www.postman.com/downloads/) for free from the [official Postman site](https://www.postman.com/). If installing software is not possible, due to Proxy issues, Company policies, or other restrictions, there is an [online version of Postman](https://blog.postman.com/announcing-postman-for-the-web-now-in-open-beta/). You sign up and it works beautifully. I highly recommend it. I did get an occasional [CORS issue](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/Errors) when my network setup was really muddy, but 99% of the time it's easy to use and also very portable - which plays an important role if you often switch between machines and environments.


Postman offers an option to save your credentials and speed up your calls, but since authentication is exactly the thing you will be testing to check your API Management setup - do not use that option.

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc15.png" width="400">




Sending the request for the JWT requires the following:
* POST
* client_id
* client_secret
* grant_type
* scope

 
 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc16.png" width="400">

Set **Grant_type** to "**client_credentials**". Make sure the scope is the same scope you defined inside your Azure app registration:

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc17.png" width="200">

 <br/>
 
 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc18.png" width="600">







<h3>Application Insights</h3>

Application Insights is an extension of Azure Monitor and provides Application Performance Monitoring (also known as “APM”) features. It is well integrated with Azure Functions with no additional coding effort. To start using Application Insights, navigate to your Function App, scroll down, and click **Turn on Application Insights**

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc19.png" width="400">


Make sure the Application Insights are also gathering logs for API Management:

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc20.png" width="400">
 
Verify your Application Insights setup by sending e.g. an expired token. You should see an Exception like this in the logs:

   <img src="/articles/images/SecureAzFunc/Github-SecAzFunc21.png" width="400">




<h4>Selected features</h4>

Some of the cool features of Application Insights are:
* OOTB Availability Tests that monitor if your app is up and running

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc22.png" width="400">

* OOTB Alerts sent by email or SMS

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc23.png" width="400">

* Ability to see an overview of general app performance and to drill down into properties of each exception (you can customize these too)

   <img src="/articles/images/SecureAzFunc/Github-SecAzFunc24.png" width="400">

* Ability to group results in order to identify patterns and recurring issues

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc25.png" width="400">







<h3>Trace</h3>
Trace is one of the test options within the API Management. It allows you to quickly test your calls. Mind you, trace logs may contain sensitive information such as keys, access tokens, passwords, internal hostnames, and IP addresses. Be careful when sharing trace logs from API Management.

<h4>How to enable it?</h4>

 <img src="/articles/images/SecureAzFunc/Github-SecAzFunc26.png" width="400">
 



<h3>Monitor executions with History</h3>
Every Azure Function needs a storage, where you will find 2 tables:

* History
* Instances

The data contains the output and input of the Azure Function, as well as the times when the Azure Function was called.

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc27.png" width="400">
 

<br/><br/>

<h1>See Also</h1>


[Authentication and authorization in Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/authentication-authorization-overview)

[Introduction to permissions and consent](https://learn.microsoft.com/en-us/azure/active-directory/develop/permissions-consent-overview)https://learn.microsoft.com/en-us/azure/active-directory/develop/permissions-consent-overview

[Just what *is* the /.default scope in the Microsoft identity platform & Azure AD?](https://dev.to/425show/just-what-is-the-default-scope-in-the-microsoft-identity-platform-azure-ad-2o4d)https://dev.to/425show/just-what-is-the-default-scope-in-the-microsoft-identity-platform-azure-ad-2o4d



<!-- Default Statcounter code for Azure - all
https://powershellscripts.github.io/articles/en/Azure/Securing%20Azure%20Functions/
-->
<script type="text/javascript">
var sc_project=12962351; 
var sc_invisible=1; 
var sc_security="ab89bc3d"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/12962351/0/ab89bc3d/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
