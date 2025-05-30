---
layout: page
title: 'SharePoint Claims Deep Dive'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-08-01'
---


# Introduction

In SharePoint Online, user and group identities are represented by different claim providers, which determine how identities are authenticated and identified within the system. SharePoint claim provider prefixes are used to format login names and indicate the type of authentication and the source of the identity claim. They help SharePoint understand how to authenticate and authorize the user or group. 

<br/><br/>

# Claim Provider Prefixes

<br/>

### c:0t.c|tenant

**Meaning**: This prefix is used for Azure AD security groups or users.

**Example**: c:0t.c\|tenant\|6510e196-d412-41de-a2e3-f99e8c0ffb4a

**Explanation**: When you see this prefix, it generally indicates a user or a group that is managed in Azure Active Directory. The prefix components explain the claim's context and source:

- **c:** - Indicates a claim.
- **0t:** - Represents the type of identity provider; "0t" usually signifies a tenant-based identity.
- **.c:** - Component indicating the claim type within the context of the tenant.
- **\|** - Separates different parts of the claim.
- **tenant** - Specifies that the identity provider is the tenant's own identity provider.


<br/>

### c:0o.c|federateddirectoryclaimprovider

**Meaning**: This prefix is used for claims originating from a federated directory claim provider.

**Example**: c:0o.c\|federateddirectoryclaimprovider\|id

**Explanation**: This prefix indicates that the claim comes from an external directory service provider that is federated with the local directory. It is commonly used in environments where identity federation is set up, allowing users from one domain to access resources in another domain.

**Prefix Components**:
The prefix components explain the claim's context and source. Here is a breakdown:

- **c:** - Indicates a claim.
- **0o:** - Represents the type of identity provider; "0o" usually signifies a federated identity provider.
- **.c:** - Component indicating the claim type within the context of the federated provider.
- **\|** - Separates different parts of the claim.
- **federateddirectoryclaimprovider** - Specifies that the identity provider is an external, federated directory service.


<br/>

### c:0-.f\|rolemanager

**Meaning**: This prefix is used for claims associated with roles managed by a role manager.

**Example**: c:0-.f\|rolemanager\|administrator

**Explanation**: This prefix indicates that the claim is related to a role that is managed by a role manager within the system. It is used to identify roles that users are assigned to, allowing for role-based access control (RBAC) within SharePoint or other systems.

**Prefix Components**:
The prefix components explain the claim's context and source. Here is a breakdown:

- **c:** - Indicates a claim.
- **0-:** - Represents a type of identity or role identifier.
- **.f:** - Component indicating the claim type is a role within the context of a role manager.
- **\|** - Separates different parts of the claim.
- **rolemanager** - Specifies that the claim is managed by a role manager.


<br/>

### c:0(.s|true

**Meaning**: This prefix represents the "Everyone" group.

**Example**: c:0(.s\|true

**Explanation**: This is used to identify the "Everyone" group in SharePoint, which includes all users who have access to the site, both authenticated and anonymous users. The prefix components explain the claim's context and source:

- **c:** - Indicates a claim.
- **0:** - Typically represents the trust level or identifier of the issuer.
- **(.s:** - Indicates a special or system claim, in this case, used for the "Everyone" group.
- **\|** - Separates different parts of the claim.
- **true** - Represents the specific claim value, indicating all users (both authenticated and anonymous).

<br/>

### i:0#.f|membership

**Meaning**: This prefix is used for standard Azure AD users.

**Example**: i:0#.f\|membership\|user@domain.com

**Explanation**: This is a common prefix for individual users authenticated via Azure AD.

<br/>

### c:0-.t|forms

**Meaning**: This prefix is used for forms-based authentication users.

**Example**: c:0-.t\|forms\|user@domain.com

**Explanation**: This indicates users who are authenticated using forms-based authentication.

<br/>

### c:0-.w|windows

**Meaning**: This prefix is used for Windows-based authentication users.

**Example**: c:0-.w\|windows\|domain\user

**Explanation**: This is used for users authenticated via Windows authentication (Kerberos/NTLM).


<br/><br/><br/>
# What Does It Depend On?
The specific claim provider prefix depends on several factors:

**Authentication Method:** How the user or group is authenticated (Azure AD, federated identity, forms-based authentication, Windows authentication, etc.).

**Identity Source:** Whether the identity is coming from Azure AD, an on-premises directory, or another identity provider.

**SharePoint Configuration:** How SharePoint is configured to handle authentication and authorization.




<br/><br/><br/>



### Prefix Components
The prefix components explain the claim's context and source. Each component in the prefix provides specific information about the type and source of the claim, helping to determine how it should be processed and applied within the SharePoint environment. The prefixes here are described after [*Understanding login name format of SharePoint* post in MS Learn](https://learn.microsoft.com/en-us/answers/questions/349797/understanding-login-name-format-of-sharepoint)



#### IdentityClaim

IdentityClaim indicates the type of claim and is the following:
* “i” for an identity claim
* “c” for any other claim

<br/>

#### ClaimType

ClaimType indicates the format for the claim value and is the following:

* “#” for a user logon name
* “.” for an anonymous user
* “5” for an email address
* “!” for an identity provider
* “+” for a Group security identifier (SID)
* “-” for a role
* “%” for a farm ID
* “?” for a name identifier
* "\" for a private personal identifier (PPID)
* "e" for a user principal name (UPN)
* """ for a user ID
* "$" for a distribution list security identifier (SID)
* "&" for a process identity security identifier (SID)
* "'" for a process identity logon name
* "(" for an authenticated user
* ")" for a primary security identifier (SID)
* "*" for a primary group security identifier (SID)
* "0" for an authorization decision
* "1" for a country
* "2" for a date of birth
* "3" for a deny only security identifier (SID)
* "4" for DNS
* "6" for a gender
* "7" for a given name
* "8" for a hash
* "9" for a home phone
* "<" for a locality
* "=" for a mobile phone
* ">" for a name
* "@" for other phone
* "[" for a postal code
* "]" for RSA
* "^" for a secure identifier (SID)
* "_" for a service principal name (SPN)
* "`" for a state or province
* "a" for a street address
* "b" for a surname
* "c" for a system
* "d" for a thumbprint
* "f" for a uniform resource name (URI)
* "g" for a web page

  <br/>
  
#### ClaimValueType

ClaimValueType indicates the type of formatting for the claim value and is the following:
- **"."** for a string
- **"+"** for an RFC 822-formatted name
- **")"** for an integer
- **"**"** for a Boolean
- **"#"** for a date
- **"$"** for a date with time
- **"&"** for a double
- **"!"** for a Base64 formatted binary
- **"0"** for an X.500 formatted name

<br/>

#### AuthMode

AuthMode indicates the type of authentication used to obtain the identity claim and is the following:

* “w” for Windows claims (no original issuer)
* “s” for the local SharePoint security token service (STS) (no original issuer)
* “t” for a trusted issuer
* “m” for a membership issuer
* “r” for a role provider issuer
* “f” for forms-based authentication
* “c” for a claim provider

<br/>

#### OriginalIssuer

OriginalIssuer indicates the original issuer of the claim.

<br/>

#### ClaimValue

ClaimValue indicates the value of the claim in the **ClaimValueType** format.



# See Also

[Claims-based identity term definitions](https://learn.microsoft.com/en-us/sharepoint/dev/general-development/claims-based-identity-term-definitions)

[Claims-based identity and concepts in SharePoint](https://learn.microsoft.com/en-us/sharepoint/dev/general-development/claims-based-identity-and-concepts-in-sharepoint)

Old but still great, check it our especially for the diagramm: [How Claims encoding works in SharePoint 2010](https://www.wictorwilen.se/blog/how-claims-encoding-works-in-sharepoint-2010/)



<!-- Default Statcounter code for Spo Claims
https://powershellscripts.github.io/articles/en/SharePointOnline/spoclaims
-->
<script type="text/javascript">
var sc_project=13024138; 
var sc_invisible=1; 
var sc_security="6a8692a5"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/13024138/0/6a8692a5/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->