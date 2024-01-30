---
layout: page
title: 'Securing Azure Functions: Scope'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-01-28'
---


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

If you receive **Application is not assigned to a role for the application** error, go to App Registrations >> Choose your client app >> API Permissions:

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

<h1>See Also</h1>

[Securing Azure Functions: Design Considerations](https://powershellscripts.github.io/articles/English/Azure/Securing%20Azure%20Functions%20-%20Design/)

[Securing Azure Functions: API Management Policies](https://powershellscripts.github.io/articles/English/Azure/Securing%20Azure%20Functions%20-%20API/)

[Securing Azure Functions: Tooling](https://powershellscripts.github.io/articles/English/Azure/Securing%20Azure%20Functions-Tooling/)


<!-- Default Statcounter code for Azure - Scope
https://powershellscripts.github.io/articles/English/Azure/Securing%20Azure%20Functions%20-%20Scope/
-->
<script type="text/javascript">
var sc_project=12962354; 
var sc_invisible=1; 
var sc_security="7644b0bc"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="free web stats"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/12962354/0/7644b0bc/1/"
alt="free web stats"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->

