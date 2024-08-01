
SharePoint Claims Deep Dive


In SharePoint Online, user and group identities are represented by different claim providers, which determine how identities are authenticated and identified within the system. The specific identifiers you mentioned, c:0t.c|tenant and c:0o.c|federateddirectoryclaimprovider, refer to different types of claims for users and groups. Here's a breakdown of what they mean and what other options might be available:

Claim Provider Prefixes
c:0t.c|tenant:

Meaning: This prefix is used for Azure AD security groups.
Example: c:0t.c|tenant|groupid
Explanation: When you see this prefix, it generally indicates a group that is managed in Azure Active Directory.
c:0o.c|federateddirectoryclaimprovider:

Meaning: This prefix is typically used for users or groups from a federated directory or claims-based authentication provider.
Example: c:0o.c|federateddirectoryclaimprovider|user@domain.com
Explanation: This prefix is used when identities are coming from a federated identity provider, such as another Azure AD tenant or an on-premises directory synchronized with Azure AD.
Other Common Claim Provider Prefixes
c:0-.f|rolemanager:

Meaning: This prefix is used for SharePoint Online roles.
Example: c:0-.f|rolemanager|sharepoint-group-name
Explanation: This prefix indicates SharePoint groups or roles, which are managed within SharePoint itself.
i:0#.f|membership:

Meaning: This prefix is used for standard Azure AD users.
Example: i:0#.f|membership|user@domain.com
Explanation: This is a common prefix for individual users authenticated via Azure AD.
c:0-.t|forms:

Meaning: This prefix is used for forms-based authentication users.
Example: c:0-.t|forms|user@domain.com
Explanation: This indicates users who are authenticated using forms-based authentication.
c:0-.w|windows:

Meaning: This prefix is used for Windows-based authentication users.
Example: c:0-.w|windows|domain\user
Explanation: This is used for users authenticated via Windows authentication (Kerberos/NTLM).
What Does It Depend On?
The specific claim provider prefix depends on several factors:

Authentication Method: How the user or group is authenticated (Azure AD, federated identity, forms-based authentication, Windows authentication, etc.).
Identity Source: Whether the identity is coming from Azure AD, an on-premises directory, or another identity provider.
SharePoint Configuration: How SharePoint Online is configured to handle authentication and authorization.
Exact Meaning and Purpose
The claim provider prefixes are used to uniquely identify the source and type of identity in SharePoint Online. They help SharePoint understand how to authenticate and authorize the user or group. For instance:

Azure AD Security Groups: c:0t.c|tenant indicates that the group is managed in Azure AD and SharePoint should query Azure AD for group membership and permissions.
Federated Identities: c:0o.c|federateddirectoryclaimprovider indicates that the identity comes from a federated directory, and SharePoint should interact with the federated identity provider for authentication and authorization.
These prefixes ensure that SharePoint can correctly resolve identities and apply the appropriate permissions and access controls based on the identity's source and type.

In SharePoint Online, claim provider prefixes are used to format login names and indicate the type of authentication and the source of the identity claim. Here are some common types and their meanings:

c:0t.c|tenant|<GUID>: This format is used for a tenant-scoped claim, typically representing Office 365 users or security groups.
c:0o.c|federateddirectoryclaimprovider|<group_guid>: This format is used for federated directory claims, such as Azure AD groups.
c:0(.s|true: Represents the "Everyone" group.
c:0-.f|rolemanager|spo-grid-all-users/<tenant_id>: Represents "Everyone except external users".
The prefix components explain the claim's context and source:

c or i: c is for claims, while i is for identity claims.
<ClaimType>: Indicates the format of the claim value, such as # for user logon names or . for anonymous users.
<ClaimValueType>: Indicates the type of the claim value, such as . for strings or + for RFC 822 formatted names.
<AuthMode>: Indicates the type of authentication, such as w for Windows claims or c for claim providers.
These claim formats help SharePoint identify and authenticate users and groups within the system​ (Microsoft Support)​​ (MS Learn)​. For more detailed information, you can refer to the Microsoft documentation on this topic.






Components explained:
<IdentityClaim>

<IdentityClaim> indicates the type of claim and is the following:
“i” for an identity claim
“c” for any other claim
<ClaimType>

<ClaimType> indicates the format for the claim value and is the following:
“#” for a user logon name
“.” for an anonymous user
“5” for an email address
“!” for an identity provider
“+” for a Group security identifier (SID)
“-“ for a role
“%” for a farm ID
“?” for a name identifier
"\" for a private personal identifier (PPID)
"e" for a user principal name (UPN)
""" for a user ID
"$" for a distribution list security identifier (SID)
"&" for a process identity security identifier (SID)
"'" for a process identity logon name
"(" for an authenticated user
")" for a primary security identifier (SID)
"*" for a primary group security identifier (SID)
"0" for an authorization decision
"1" for a country
"2" for a date of birth
"3" for a deny only security identifier (SID)
"4" for DNS
"6" for a gender
"7" for a given name
"8" for a hash
"9" for a home phone
"<" for a locality
"=" for a mobile phone
">" for a name
"@" for other phone
"[" for a postal code
"]" for RSA
"^" for a secure identifier (SID)
"_" for a service principal name (SPN)
"`" for a state or province
"a" for a street address
"b" for a surname
"c" for a system
"d" for a thumbprint
"f" for a uniform resource name (URI)
"g" for a web page
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
