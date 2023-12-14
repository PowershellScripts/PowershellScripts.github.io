Why?

This tip is particularly relevant if you need to allow management of custom content types for all lists in your site or you need to limit the use of custom content types for lists.  This article presents a quick, script-based solution, 

Content types

Content types help you customize how you handle and track specific kinds of content. Content types also make it possible for a single list or library to contain multiple item types or document types. If you have specific types of documents that have a standardized format or purpose, e.g. Equipment Request, Purchase Order, or Sales Contract and you want them to be consistent across your organization, content types enable site users to quickly create those specialized kinds of content by using the New Item or New Document command in a list or library.1 Jump  Site owners can pre-configure specific details Jump about the content when they set up content types for a site, list, or library.1 Jump

By default, SharePoint includes many content types. In fact, every piece of content in SharePoint is created from a content type. There are pre-defined content types, such as Blank Document or Announcement, however, every organization has unique requirements and different kinds of content. Formal contracts, reports, sales proposals, design specifications, company procedures may all require you to create separate content types to manage and standardize this variety.2 Jump
The content type settings allow you to specify such settings as columns, display forms, workflows or document templates, but before adding content types on a library or list, it is helpful to understand the following:3 Jump

Content types can be configured to require that certain fields, also known as columns, contain information. When uploading or creating a document, if no information is included in the required columns, the user is prompted to provide it. The required columns are configured on and enforced by the content type, not by the list or library.

The fields that appear on a form are determined by the content type associated with that form, not by the list or library.

The document template that is used when creating a new document is specified by the content type, not the list or library.

Workflows and events can be bound to content types.

Content types that have a parent/child relationship with a higher level site can be affected by actions that occur when the parent content type is updated.

From Multiple content type considerations Jump

User Interface

How to disable/enable content type management manually:

1. Open the list or library where you want to change the setting.

2. Click on the List or Library Tab.
 


3. On the Ribbon click List Settings.
 

4. In the Settings choose Advanced settings.
 

5. Under Content types section, you can choose whether to allow or not the management of content types. Yes corresponds to $true in the script below, while No corresponds to $false:
 

PowerShell
Prerequisites
PowerShell ISE

SharePoint Online SDK. 
Jump
For detailed instruction with screenshots on how to install SharePoint Online SDK and run Powershell ISE please refer here.

All lists

This section presents how to enable content type management for all lists in a given site.
In the ISE Window, add the paths to SharePoint Online SDK installed earlier. Verify if the paths are exactly the same, e.g. on Windows Server, you may need to change 15 into 16:
  

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

Enter the credentials and the site where you want to change the content types management setting

$Username="trial@trialtrial123.onmicrosoft.com"
$AdminPassword="Pass"
$Url="https://trialtrial123.sharepoint.com/sites/teamsitewithlists Jump "

It is a good practice to keep all the information that a user may change in one place, instead of spreading it across the whole code for us to search whenever we want to re-use it. So let's decide now whether we want to allow content type management or disable the support for content types and store this decision as a bool variable:

$ContentTypesEnabled=$true
As you see, I have decided to enable the support for content types in my lists.
 
Now let's do something with the data we provided. For most of the actions, it is useful to put them in the form of a function. Functions are blocks of code designated to perform a particular task. If we encapsulate our cmdlets in such a block, it will be very easy to re-use it later on and that's what automation is all about:

function Set-SPOListsContentTypesEnabled
{
 
}

The function can take parameters, defined either in ( ) next to the function name or in a way shown below:
param (
  [Parameter(Mandatory=$true,Position=1)]
        [string]$Username,
        [Parameter(Mandatory=$true,Position=2)]
        [string]$AdminPassword,
        [Parameter(Mandatory=$true,Position=3)]
        [string]$Url,
        [Parameter(Mandatory=$true,Position=4)]
        [bool]$ContentTypesEnabled
)


Password. We can either take the password from the input earlier 
$password = ConvertTo-SecureString -string $AdminPassword -AsPlainText -Force

or if you are concerned with security - ask for it every time the function will run (that's once per site).
$password=Read-Host -Prompt "Enter password"  -AsSecureString

Let's create the context now for our actions. We will be operating within the site/ site collection we defined earlier in the $Url variable 
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)

Add the credentials to the context, so that we can authenticate.
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $password)

.ExecuteQuery() isn't necessary at this stage, but I like to run it early to make sure that authentication ran smoothly. If there is an error in the $Url, $Username, $password, or the site doesn't exist, it will throw us an error and we can narrow down our troubleshooting scope.
$ctx.ExecuteQuery()

I want to apply the setting to all lists, so first I have to retrieve them. 
$Lists=$ctx.Web.Lists
 
$ctx.Load($Lists)
 
$ctx.ExecuteQuery()
 
In order to change the setting for every list, we have to loop through them. We can use for loop Jump or foreach loop Jump in this case. 
Foreach($ll in $Lists)
 
{
    
 
}
 
$ll represents the list at which my loop currently arrived. Set the property "Attachments" of the list $ll (current list)  to the value hidden under variable $Attachments (in our case it is $false). 
$ll.ContentTypesEnabled = $ContentTypesEnabled

If you didn't specify the $ContentTypesEnabled=$true variable earlier and in the parameters of the function, you could just write
$ll.ContentTypesEnabled = $true


Update the list with the new settings.
$ll.Update()


Send this update for execution.
$ctx.ExecuteQuery()


.ExecuteQuery() is likely to throw us an error, so let's encapsulate it with try and catch.
try

{

$ctx.ExecuteQuery()

 

}

catch [Net.WebException]

{

 

 

 

}

 
If something goes wrong, the code will probably continue, because in Powershell only terminating errors terminate the code execution. When the ThrowTerminatingError() method is called, the Windows PowerShell runtime permanently stops the execution of the pipeline and throws a PipelineStoppedException exception. Any subsequent attempts to call WriteObject, WriteError, or several other APIs causes those calls to throw a PipelineStoppedException exception.3 Otherwise, the code proceeds to the next object. Cmdlets can write any number of output objects or non-terminating errors before reporting a terminating error. However, the terminating error permanently stops the pipeline, and no further output, terminating errors, or non-terminating errors can be reported.3 I would like to know what went wrong, so let's add Write-Host:

try

{

$ctx.ExecuteQuery()

Write-Host $ll.Title " Done" -ForegroundColor Green

}

catch [Net.WebException]

{

 

Write-Host "Failed" $_.Exception.ToString() -ForegroundColor Red

}


The only remaining thing is to call the function:
Set-SPOListsContentTypesEnabled -Username $Username -AdminPassword $AdminPassword -Url $Url -ContentTypesEnabled $ContentTypesEnabled


If you decided to skip the $password or $Attachments parameters (for security and ease reasons), don't forget to modify the parameters of the functions
param (

[Parameter(Mandatory=$true,Position=1)]

[string]$Username,

[Parameter(Mandatory=$true,Position=3)]

[string]$Url,

)

 
The Position attribute of the parameters is optional, and the positions don't have to be contiguous. Positions 5, 100, and 250 work the same as positions 0, 1, and 2.4

The cmdlet will look different as well:
Set-SPOListsContentTypesEnabled -Username $Username -Url $Url


All lists without blabbing
## Created by Someone Awesome, 2015#

function Set-SPOListsContentTypesEnabled

{

param (

[Parameter(Mandatory=$true,Position=1)]

[string]$Username,

[Parameter(Mandatory=$true,Position=2)]

[string]$AdminPassword,

[Parameter(Mandatory=$true,Position=3)]

[string]$Url,

[Parameter(Mandatory=$true,Position=4)]

[bool]$ContentTypesEnabled

)

$password = ConvertTo-SecureString -string $AdminPassword -AsPlainText -Force

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)

$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $password)

$ctx.ExecuteQuery()

$Lists=$ctx.Web.Lists

$ctx.Load($Lists)

$ctx.ExecuteQuery()

Foreach($ll in $Lists)

{

$ll.ContentTypesEnabled = $ContentTypesEnabled

$ll.Update()

 

try

{

$ctx.ExecuteQuery()

Write-Host $ll.Title " Done" -ForegroundColor Green

}

catch [Net.WebException]

{

 

Write-Host "Failed" $_.Exception.ToString() -ForegroundColor Red

}

}

}

　

# Paths to SDK. Please verify location on your computer.

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

# Insert the credentials and the name of the site and the desired setting: $true for the content types management to be allowed or $false to disable it

$Username="trial@trialtrial123.onmicrosoft.com"

$AdminPassword="Pass"

$Url="https://trialtrial123.sharepoint.com/sites/teamsitewithlists"

$ContentTypesEnabled=$true


Set-SPOListsContentTypesEnabled -Username $Username -AdminPassword $AdminPassword -Url $Url -ContentTypesEnabled $ContentTypesEnabled


One list

This section presents how to enable content type management for a single list of your choice. I decided to use 2 functions for that purpose. There are, of course, multiple approaches available and this is only one of them.

Declare variables
As with changing the setting for all lists, let us first declare all data so that it is stored in one place. Notice that this time we need to enter the name of the list as we will be performing operations only on this list.
# Paths to SDK. Please verify location on your computer.

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

# Insert the credentials and the name of the site and list

$Username="trial@trialtrial123.onmicrosoft.com"

$AdminPassword="Pass"

$Url="https://trialtrial123.sharepoint.com/sites/teamsitewithlists"

$ListName="Tasks list"

$ContentTypesEnabled =$false


Again, I am using only a test tenant, but if you are concerned with safety reasons, instead of entering the password in a form of a string, ask the user to enter the password as a SecureString each time the script runs:
$password=Read-Host "Enter password" -AsSecureString


Authentication function
The first function deals with authentication. It will create a context which later can be reused by multiple functions/cmdlets. The context will be stored in a global variable Jump for that purpose. The benefit of such function can be seen e.g. in SharePoint Module for lists, items and files Jump , where after you have run Connect-SPOCSOM cmdlet once, you do not need to re-enter the credentials for all other cmdlets.
Again,   param ()  defines the  parameters  that will be used later on in a function:

param (

[Parameter(Mandatory=$true,Position=1)]

 

[string]$Username,

 

[Parameter(Mandatory=$true,Position=2)]

 

[string]$AdminPassword,

 

[Parameter(Mandatory=$true,Position=2)]

 

[string]$Url

 

)

Password. We can either take password from the input earlier 

$password = ConvertTo-SecureString -string $AdminPassword -AsPlainText -Force

 or
$password=Read-Host -Prompt "Enter password"  -AsSecureString


The context now for our actions - site/ site collection for our operations, as defined in the $Url  variable
$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)

Credentials for authentication
$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $password)

.ExecuteQuery() isn't necessary at this stage, but I like to run it early to make sure that authentication ran smoothly. If there is an error in the $Url, $Username, $password, or the site doesn't exist, it will throw us an error and we can narrow down our troubleshooting scope.
$ctx.ExecuteQuery()

Full function:

function Connect-SPOCSOM

{

param (

[Parameter(Mandatory=$true,Position=1)]

[string]$Username,

[Parameter(Mandatory=$true,Position=2)]

[string]$AdminPassword,

[Parameter(Mandatory=$true,Position=2)]

[string]$Url

)

$password = ConvertTo-SecureString -string $AdminPassword -AsPlainText -Force

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)

$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $password)

$ctx.ExecuteQuery()

$global:ctx=$ctx

}


Setting function

This functions sets the property of the list. Powershell cmdlet follow standard Verb-Noun pattern, so let's name accordingly:

function Set-SPOList

{

 

 

 

 

 

}


Define the parameters:

param (

[Parameter(Mandatory=$true,Position=0)]

[string]$ListName,

[Parameter(Mandatory=$true,Position=1)]

[bool]$ContentTypesEnabled

)


Find the list:
$ll=$ctx.Web.Lists.GetByTitle($ListName)

 

Update its properties:
$ll.ContentTypesEnabled = $ContentTypesEnabled

$ll.Update()


.ExecuteQuery() using try and catch
try

{

$ctx.ExecuteQuery()

Write-Host "Done" -ForegroundColor Green

}

catch [Net.WebException]

{

 

Write-Host "Failed" $_.Exception.ToString() -ForegroundColor Red

}

 

Full cmdlet:
function Set-SPOList

{

param (

[Parameter(Mandatory=$true,Position=0)]

[string]$ListName,

[Parameter(Mandatory=$true,Position=1)]

[bool]$ContentTypesEnabled

)

$ll=$ctx.Web.Lists.GetByTitle($ListName)

$ll.ContentTypesEnabled = $ContentTypesEnabled

$ll.Update()

try

{

$ctx.ExecuteQuery()

Write-Host "Done" -ForegroundColor Green

}

catch [Net.WebException]

{

 

Write-Host "Failed" $_.Exception.ToString() -ForegroundColor Red

}

 

}


One list without blabbing
## Created by Someone Awesome, 2015#

function Set-SPOList

{

 

param (

[Parameter(Mandatory=$true,Position=0)]

[string]$ListName,

[Parameter(Mandatory=$true,Position=1)]

[bool]$ContentTypesEnabled

 

)

 

$ll=$ctx.Web.Lists.GetByTitle($ListName)

$ll.ContentTypesEnabled = $ContentTypesEnabled

$ll.Update()

try

 

{

$ctx.ExecuteQuery()

Write-Host "Done" -ForegroundColor Green

 

}

 

catch [Net.WebException]

 

{

Write-Host "Failed" $_.Exception.ToString() -ForegroundColor Red

 

}

}

 

function Connect-SPOCSOM

{

 

param (

[Parameter(Mandatory=$true,Position=1)]

[string]$Username,

[Parameter(Mandatory=$true,Position=2)]

[string]$AdminPassword,

[Parameter(Mandatory=$true,Position=2)]

[string]$Url

 

)

 

$password = ConvertTo-SecureString -string $AdminPassword -AsPlainText -Force

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)

$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $password)

$ctx.ExecuteQuery()

$global:ctx=$ctx

 

}

 

 

$global:ctx

 

# Paths to SDK. Please verify location on your computer.

 

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

 

# Insert the credentials and the name of the site and list

 

$Username="trial@trialtrial123.onmicrosoft.com"

$AdminPassword="Pass"

$Url="https://trialtrial123.sharepoint.com/sites/teamsitewithlists"

$ListName="Tasks list"

$ContentTypesEnabled =$false

 

Connect-SPOCSOM -Username $Username -AdminPassword $AdminPassword -Url $Url

Set-SPOList -ListName $ListName -ContentTypesEnabled $ContentTypesEnabled

 

All lists in all sites in a site collection

Comments

The script is similar to the other two and therefore for the detailed description of every line please refer to  All Lists  or One List
In order to loop through all sites and their subsites, it uses recurrence. More efficient would be -Recursive parameter like in list items, but it doesn't seem to be available for sites.
After authentication, we load not only lists, but also the site and its subsites (Web.Webs):
$password = ConvertTo-SecureString -string $AdminPassword -AsPlainText -Force

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)

$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $password)

$ctx.ExecuteQuery()

$Lists=$ctx.Web.Lists

$ctx.Load($ctx.Web)

$ctx.Load($ctx.Web.Webs)

$ctx.Load($Lists)

$ctx.ExecuteQuery()


The recursion Jump :

for($i=0;$i -lt $ctx.Web.Webs.Count ;$i++)

{

Set-SPOListsContentTypesEnabled -Username $Username -Url $ctx.Web.Webs[$i].Url -AdminPassword $AdminPassword -ContentTypesEnabled $ContentTypesEnabled

}

In order to notify about start of a new site, add a simple  Write-Host line:

Write-Host "--"-ForegroundColor DarkGreen

It doesn't make sense to loop through the sites if there aren't any, so verify this condition in the beginning:

if($ctx.Web.Webs.Count -gt 0)


Full section for the subsites:
if($ctx.Web.Webs.Count -gt 0)

{

Write-Host "--"-ForegroundColor DarkGreen

for($i=0;$i -lt $ctx.Web.Webs.Count ;$i++)

{

Set-SPOListsContentTypesEnabled -Username $Username -Url $ctx.Web.Webs[$i].Url -AdminPassword $AdminPassword -ContentTypesEnabled $ContentTypesEnabled

}

}


Script
#
# Created by Arleta Wanat, 2015

#

function Set-SPOListsContentTypesEnabled

{

param (

[Parameter(Mandatory=$true,Position=1)]

[string]$Username,

[Parameter(Mandatory=$true,Position=2)]

[string]$AdminPassword,

[Parameter(Mandatory=$true,Position=3)]

[string]$Url,

[Parameter(Mandatory=$true,Position=4)]

[bool]$ContentTypesEnabled

)

$password = ConvertTo-SecureString -string $AdminPassword -AsPlainText -Force

$ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)

$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $password)

$ctx.ExecuteQuery()

$Lists=$ctx.Web.Lists

$ctx.Load($ctx.Web)

$ctx.Load($ctx.Web.Webs)

$ctx.Load($Lists)

$ctx.ExecuteQuery()

Foreach($ll in $Lists)

{

$ll.ContentTypesEnabled = $ContentTypesEnabled

$ll.Update()

 

try

{

$ctx.ExecuteQuery()

Write-Host $ll.Title " Done" -ForegroundColor Green

}

catch [Net.WebException]

{

 

Write-Host "Failed" $_.Exception.ToString() -ForegroundColor Red

}

}

if($ctx.Web.Webs.Count -gt 0)

{

Write-Host "--"-ForegroundColor DarkGreen

for($i=0;$i -lt $ctx.Web.Webs.Count ;$i++)

{

Set-SPOListsContentTypesEnabled -Username $Username -Url $ctx.Web.Webs[$i].Url -AdminPassword $AdminPassword -ContentTypesEnabled $ContentTypesEnabled

}

}

 

}


# Paths to SDK. Please verify location on your computer.

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

# Insert the credentials and the name of the site and the desired setting: $true for the content types management to be allowed or $false to disable it

$Username="trial@trialtrial123.onmicrosoft.com"

$AdminPassword="Pass"

$Url="https://trialtrial123.sharepoint.com/sites/teamsitewithlists"

$ContentTypesEnabled=$true


Set-SPOListsContentTypesEnabled -Username $Username -AdminPassword $AdminPassword -Url $Url -ContentTypesEnabled $ContentTypesEnabled


Results

The script changes the setting for all lists in all subsites, sub-subsites, sub-sub-subsites, and so on.  Not all lists can support content type management, e.g. User Information List. If the list does not support it, you will receive an error below the list name. Every subsequent site is separated by "--".
 

Download
