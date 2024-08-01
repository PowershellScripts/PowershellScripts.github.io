---
layout: page
title: 'SharePoint Claims Deep Dive'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-08-01'
---


# Introduction

In SharePoint Online, user and group identities are represented by different claim providers, which determine how identities are authenticated and identified within the system. SharePoint claim provider prefixes are used to format login names and indicate the type of authentication and the source of the identity claim. They help SharePoint understand how to authenticate and authorize the user or group. 

<br/><br/>

# Claim Provider Prefixes

<br/>

### c:0t.c|tenant:

**Meaning**: This prefix is used for Azure AD security groups or users.

**Example**: c:0t.c|tenant|6510e196-d412-41de-a2e3-f99e8c0ffb4a

**Explanation**: When you see this prefix, it generally indicates a user or a group that is managed in Azure Active Directory. The prefix components explain the claim's context and source:

- **c:** - Indicates a claim.
- **0t:** - Represents the type of identity provider; "0t" usually signifies a tenant-based identity.
- **.c:** - Component indicating the claim type within the context of the tenant.
- **|** - Separates different parts of the claim.
- **tenant** - Specifies that the identity provider is the tenant's own identity provider.


<br/>

### c:0o.c|federateddirectoryclaimprovider:

**Meaning**: This prefix is typically used for users or groups from a federated directory or claims-based authentication provider.

**Example**: c:0o.c|federateddirectoryclaimprovider|user@domain.com

**Explanation**: This prefix is used when identities are coming from a federated identity provider, such as another Azure AD tenant or an on-premises directory synchronized with Azure AD.

<br/>

### c:0-.f|rolemanager:

**Meaning**: This prefix is used for SharePoint Online roles.

**Example**: c:0-.f|rolemanager|spo-grid-all-users/<tenant_id>: Represents "Everyone except external users".

**Explanation**: This prefix indicates SharePoint groups or roles, which are managed within SharePoint itself.

<br/>

### c:0(.s|true:

**Meaning**: This prefix represents the "Everyone" group.

**Example**: c:0(.s|true

**Explanation**: This is used to identify the "Everyone" group in SharePoint, which includes all users who have access to the site, both authenticated and anonymous users. The prefix components explain the claim's context and source:

- **c:** - Indicates a claim.
- **0:** - Typically represents the trust level or identifier of the issuer.
- **(.s:** - Indicates a special or system claim, in this case, used for the "Everyone" group.
- **|** - Separates different parts of the claim.
- **true** - Represents the specific claim value, indicating all users (both authenticated and anonymous).

<br/>

### i:0#.f|membership:

**Meaning**: This prefix is used for standard Azure AD users.

**Example**: i:0#.f|membership|user@domain.com

**Explanation**: This is a common prefix for individual users authenticated via Azure AD.

<br/>

### c:0-.t|forms:

**Meaning**: This prefix is used for forms-based authentication users.

**Example**: c:0-.t|forms|user@domain.com

**Explanation**: This indicates users who are authenticated using forms-based authentication.

<br/>

### c:0-.w|windows:

**Meaning**: This prefix is used for Windows-based authentication users.

**Example**: c:0-.w|windows|domain\user

**Explanation**: This is used for users authenticated via Windows authentication (Kerberos/NTLM).


<br/><br/><br/>
# What Does It Depend On?
The specific claim provider prefix depends on several factors:

**Authentication Method:** How the user or group is authenticated (Azure AD, federated identity, forms-based authentication, Windows authentication, etc.).

**Identity Source:** Whether the identity is coming from Azure AD, an on-premises directory, or another identity provider.

**SharePoint Configuration:** How SharePoint is configured to handle authentication and authorization.




<br/><br/><br/>



### Prefix Components
The prefix components explain the claim's context and source. Each component in the prefix provides specific information about the type and source of the claim, helping to determine how it should be processed and applied within the SharePoint environment.



#### IdentityClaim

IdentityClaim indicates the type of claim and is the following:
* “i” for an identity claim
* “c” for any other claim

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

  
<ClaimValueType>

<ClaimValueType> indicates the type of formatting for the claim value and is the following:
“.” for a string
“+” for an RFC 822-formatted name
")" for an integer
""" for a Boolean
"#" for a date
"$" for a date with time
"&" for a double
"!" for a Base64 formatted binary
"0" for a X.500 formatted name
<AuthMode>

<AuthMode> indicates the type of authentication used to obtain the identity claim and is the following:
“w” for Windows claims (no original issuer)
“s” for the local SharePoint security token service (STS) (no original issuer)
“t” for a trusted issuer
“m” for a membership issuer
“r” for a role provider issuer
“f” for forms-based authentication
“c” for a claim provider
<OriginalIssuer>

<OriginalIssuer> indicates the original issuer of the claim.
<ClaimValueType>

<ClaimValueType> indicates the value of the claim in the <ClaimType> format.
Common types of login names in SharePoint online:
Everyone -> c:0(.s|true
Everyone except external users -> c:0-.f|rolemanager|spo-grid-all-users/<tenant_id>
Group memebers -> c:0o.c|federateddirectoryclaimprovider|<group_guid>
Group Owners -> c:0o.c|federateddirectoryclaimprovider|<group_guid>
"Company Administrator" in Sharepoint Admin console -> c:0t.c|tenant|<UNKNOWN-GUID>
An O365 user ->i:0#.f|membership|<USER-EMAIL>
