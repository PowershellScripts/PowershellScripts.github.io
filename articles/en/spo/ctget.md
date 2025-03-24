---
layout: page
title: 'List SharePoint content types'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-03-15'
---



# What is a content type?

A Content Type in SharePoint is a reusable collection of metadata, templates, and settings that define the structure and behavior of a specific type of content across a site or library. It allows organizations to standardize and manage information consistently. For example, a "Contract" content type might include specific metadata fields like "Contract Number," "Expiration Date," and "Client Name," along with a document template. By associating content types with lists or libraries, users can easily create and manage content that aligns with organizational standards, improving consistency, compliance, and searchability. Content types are foundational for building efficient information architectures in SharePoint.


<img src="/articles/img/ctget1.png" width="600">


This article shows you how to get all or selected content types that you have in your list or your SharePoint site.

<br/><br/>

# List or Library


### User Interface - Browser

Navigate to Library or List Settings. There you will see Content Types available for this library or list. 


<img src="/articles/img/ctget2.png" width="600">



If the Content Types section is completely missing from your view, make sure to [enable content type management](https://powershellscripts.github.io/articles/en/spo/enablect/) first.



<br/><br/>

### PnP Powershell - Get all content types

```powershell

# Import the PnP PowerShell module
Import-Module PnP.PowerShell

# Connect to the SharePoint site
$SiteUrl = "https://yourtenant.sharepoint.com/sites/yoursite"
Connect-PnPOnline -Url $SiteUrl -Interactive

# Specify the list name
$ListName = "YourListName"

# Get the list
$List = Get-PnPList -Identity $ListName

# Get all content types associated with the list
$ContentTypes = Get-PnPContentType -List $List

# Display content types
$ContentTypes | ForEach-Object {
    Write-Output "Content Type Name: $($_.Name)"
    Write-Output "Content Type ID: $($_.Id.StringValue)"
    Write-Output "------------------------------"
}

# Disconnect from the site
Disconnect-PnPOnline

```

<br/>


### PnP Powershell - Get a content type by id

```powershell

# Import the PnP PowerShell module
Import-Module PnP.PowerShell

# Connect to the SharePoint site
$SiteUrl = "https://yourtenant.sharepoint.com/sites/yoursite"
Connect-PnPOnline -Url $SiteUrl -Interactive

# Specify the content type ID
$ContentTypeId = "0x0101001234567890ABCDEF1234567890ABCDEF" # Replace with your Content Type ID

# Get the content type by ID
$ContentType = Get-PnPContentType | Where-Object { $_.Id.StringValue -eq $ContentTypeId }

# Check if content type is found
if ($ContentType) {
    Write-Output "Content Type Found:"
    Write-Output "Name: $($ContentType.Name)"
    Write-Output "ID: $($ContentType.Id.StringValue)"
} else {
    Write-Output "Content Type with ID $ContentTypeId not found."
}

# Disconnect from the site
Disconnect-PnPOnline

```

<br/>

### PnP Powershell - Get a content type by name


```powershell

# Import the PnP PowerShell module
Import-Module PnP.PowerShell

# Connect to the SharePoint site
$SiteUrl = "https://yourtenant.sharepoint.com/sites/yoursite"
Connect-PnPOnline -Url $SiteUrl -Interactive

# Specify the content type name
$ContentTypeName = "Your Content Type Name" # Replace with the name of your content type

# Get the content type by name
$ContentType = Get-PnPContentType | Where-Object { $_.Name -eq $ContentTypeName }

# Check if content type is found
if ($ContentType) {
    Write-Output "Content Type Found:"
    Write-Output "Name: $($ContentType.Name)"
    Write-Output "ID: $($ContentType.Id.StringValue)"
} else {
    Write-Output "Content Type with name '$ContentTypeName' not found."
}

# Disconnect from the site
Disconnect-PnPOnline


```

<br/><br/>


# Site


### User Interface (Browser)

Navigate to Site Settings.

<img src="/articles/img/ctget3.png" width="600">

Open the Site Settings and under Web Designer Galleries you will find a link to a list of your site content types.


<img src="/articles/img/ctget4.png" width="600">


<br/><br/>

### PnP Powershell - Get all content types


```powershell

# Import the PnP PowerShell module
Import-Module PnP.PowerShell

# Connect to the SharePoint site
$SiteUrl = "https://yourtenant.sharepoint.com/sites/yoursite"
Connect-PnPOnline -Url $SiteUrl -Interactive

# Get all content types from the site
$ContentTypes = Get-PnPContentType

# Display all content types
$ContentTypes | ForEach-Object {
    Write-Output "Name: $($_.Name)"
    Write-Output "ID: $($_.Id.StringValue)"
    Write-Output "Description: $($_.Description)"
    Write-Output "------------------------------"
}

# Disconnect from the site
Disconnect-PnPOnline

```

<br/>


### CSOM - Get a content type by id

```powershell

# Load the SharePoint CSOM Assemblies
Add-Type -Path "C:\Path\To\Microsoft.SharePoint.Client.dll"
Add-Type -Path "C:\Path\To\Microsoft.SharePoint.Client.Runtime.dll"

# Define credentials
$SiteUrl = "https://yourtenant.sharepoint.com/sites/yoursite"
$Username = "yourusername@yourtenant.onmicrosoft.com"
$Password = "yourpassword"
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
$Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $SecurePassword)

# Create the ClientContext
$Context = New-Object Microsoft.SharePoint.Client.ClientContext($SiteUrl)
$Context.Credentials = $Credentials

# Specify the Content Type ID
$ContentTypeId = "0x0101001234567890ABCDEF1234567890ABCDEF" # Replace with your Content Type ID

# Get the Content Type by ID
$ContentType = $Context.Web.ContentTypes.GetById($ContentTypeId)
$Context.Load($ContentType)
$Context.ExecuteQuery()

# Check if the Content Type was found and display its details
if ($ContentType -ne $null) {
    Write-Output "Content Type Found:"
    Write-Output "Name: $($ContentType.Name)"
    Write-Output "ID: $($ContentType.Id.StringValue)"
    Write-Output "Description: $($ContentType.Description)"
} else {
    Write-Output "Content Type with ID $ContentTypeId not found."
}

```

<br/>
<br/>

# See Also

[Enable content type management](https://powershellscripts.github.io/articles/en/spo/enablect/)

[Add content type using Powershell and CSOM](https://powershellscripts.github.io/articles/en/SharePointOnline/Add%20content%20type/)


[Find content type ID](https://powershellscripts.github.io/articles/en/SharePointOnline/findctid/)