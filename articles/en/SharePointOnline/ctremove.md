---
layout: page
title: 'Content Type is still in use - How to remove stuck site content type'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-03-09'
---


# Issue 


You navigate to SharePoint List -> List Settings -> Scroll down to content types -> Open a content type

<img src="/articles/img/ctremove0.png" width="200">


Click delete this content type and receive the following error message:

<img src="/articles/img/ctremove.png" width="200">


Solution

We did the Thinker pose, shouted loudly %#$%#% (still no improvement) and resorted to something that almost never fails - PowerShell.

To save you from hurting your fists, we are sharing the solution below:

We have already PowerShell ISE and SharePoint Online SDK installed. So in the ISE Window let's go right away and add the paths to the SDK:

Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
 
Add-Type -Path "c:\Program Files\Common Files\microsoft shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

Enter the data of the site:

$Username="mszasz@trial.onmicrosoft.com"
 
$AdminPassword=Read-Host -Prompt "Password" -AsSecureString
 
$AdminUrl="https://trial.sharepoint.com/sites/firstsite"

and the content type id.

How to find the content type id?
1. Navigate to Site>Site settings > Content types

or

Enter the URL of the site where you want to remove the content type. At the end of the URL add /_layouts/15/mngctype.aspx 

At the end of step 1, you should have: https://trialtrial125.sharepoint.com/_layouts/15/mngctype.aspx

2. Click on the content type you want to remove.


3. In the browser's URL bar you will see something like:

https://trialtrial125.sharepoint.com/_layouts/15/start.aspx#/_layouts/15/ManageContentType.aspx?ctype=0x0101009148F5A04DDD49CBA7127AADA5FB792B006973ACD696DC4858A76371B2FB2F439A&Source=https%3A%2F%2Ftrialtrial125%2Esharepoint%2Ecom%2F%5Flayouts%2F15%2Fmngctype%2Easpx

The numbers marked in red are the id of the content type (not the same as the GUID). More reading content type id: Content Type IDs
This content type id is:

$ContentTypeID="0x00A7470EADF4194E2E9ED1031B61DA0884030065B86AF41E46E8408DF47ED47A09578701"

The function itself:


function Remove-Contenttype
 
{
 
param (
 
  [Parameter(Mandatory=$true,Position=1)]
 
        [string]$Username,
 
        [Parameter(Mandatory=$true,Position=2)]
 
        $AdminPassword,
 
        [Parameter(Mandatory=$true,Position=3)]
 
        [string]$Url,
 
[Parameter(Mandatory=$true,Position=4)]
 
        $ContentTypeID
 
  
 
)
 
#$password = ConvertTo-SecureString -string $AdminPassword -AsPlainText -Force
 
  $ctx=New-Object Microsoft.SharePoint.Client.ClientContext($Url)
 
  $ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $AdminPassword)
 
  $ctx.ExecuteQuery()
 
  
 
 $ctx.Load($ctx.Web)
 
  $ct=$ctx.Web.ContentTypes
 
$ctx.Load($ct)
 
$ctx.ExecuteQuery()
 
Write-Host $ctx.Web.Url $ct.Count.ToString()
 
$ctx.Web.ContentTypes.GetById($ContentTypeID).DeleteObject()
 
$ctx.ExecuteQuery()

}

All that remains is to run the cmdlet (still in the .ps1 file!)

Remove-Contenttype -Username $Username -AdminPassword $AdminPassword -Url $AdminUrl -ContentTypeID $ContentTypeID

The full script can be downloaded here.

Run the script in any PowerShell. It should ask you for your admin password.

Results

 

Some of the content types were removed in the first go (Yey!). Additionally received the site URL from where they were removed and how many content types remained (just as a security precaution).

In some of the cases, though, it failed. As a consolation we received a meaningful message from PowerShell:
"Content type MYNAME at sites/MYSITENAME is read only"

A short trip to the user interface solved the issue:

1. Site settings
2. Site content types
3. Click on the name of the content type
4. Advanced settings
5. Should this content type be read only? Well, NO
Deleted the content type from the User Interface (sorry PowerShell for betrayal...)
 





Download
The script is available also for download from the TechNet Script Gallery:

Remove content type from SharePoint site using PowerShell
