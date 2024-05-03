---
layout: page
title: 'Securing Azure Functions: API Management Policies'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-01-27'
---


<h1>API Management Policies</h1>

Azure API Management is a hybrid, multi-cloud management platform for APIs across all environments. As a platform-as-a-service, API Management supports the complete API lifecycle. The inbound processing rules allow you to configure a JWT validation policy to pre-authorize requests:
 
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
 



<h1>See Also</h1>

[Securing Azure Functions: Design Considerations](https://powershellscripts.github.io/articles/en/Azure/Securing%20Azure%20Functions%20-%20Design/)

[Securing Azure Functions: Scope](https://powershellscripts.github.io/articles/en/Azure/Securing%20Azure%20Functions%20-%20Scope/)

[Securing Azure Functions: Tooling](https://powershellscripts.github.io/articles/en/Azure/Securing%20Azure%20Functions-Tooling/)



<!-- Default Statcounter code for Azure - API
https://powershellscripts.github.io/articles/en/Azure/Securing%20Azure%20Functions%20-%20API/
-->
<script type="text/javascript">
var sc_project=12962355; 
var sc_invisible=0; 
var sc_security="042aeb11"; 
var scJsHost = "https://";
document.write("<sc"+"ript type='text/javascript' src='" +
scJsHost+
"statcounter.com/counter/counter.js'></"+"script>");
</script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/12962355/0/042aeb11/0/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->


