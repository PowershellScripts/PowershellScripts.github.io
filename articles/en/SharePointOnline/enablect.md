---
layout: page
title: 'Enable content type management'
hero_image: '/img/IMG_20220521_140146.jpg'

show_sidebar: false
hero_height: is-small
date: '2024-08-03'
---


## Why Enable Content Type Management?

Enabling content type management is crucial for customizing how specific types of content are handled and tracked within your SharePoint site. It’s especially useful if you need to:

* Allow or limit the use of custom content types across lists and libraries.
* Standardize document types like Equipment Requests, Purchase Orders, or Sales Contracts across your organization.

Content types in SharePoint help organize and manage various document types or item types in a list or library. They allow site owners to define specific fields, workflows, templates, and forms for consistent and efficient content creation.

By default, SharePoint includes several predefined content types. However, to meet your organization's unique needs, you may need to create custom content types for specific document types or workflows.



## User Interface

How to disable/enable content type management manually:

1. Open the list or library where you want to change the setting.


2. On the right-hand side click List Settings.
 
 <img src="/articles/img/enablect.PNG" ><br/>


4. In the Settings choose Advanced settings.
 
  <img src="/articles/img/enablect2.PNG" ><br/>


5. Under Content types section, you can choose whether to allow or not the management of content types. Yes corresponds to $true in the script below, while No corresponds to $false:
 

 <img src="/articles/img/enablect3.PNG" ><br/>





## PnP Powershell


### For one list

```powershell
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -UseWebLogin
Set-PnPList -Identity "Documents" -ContentTypesEnabled $true
```


### For all lists in the site collection

```powershell
# Connect to the SharePoint site
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/yoursite" -UseWebLogin

# Get all lists from the site
$lists = Get-PnPList

# Loop through each list and enable content types
foreach ($list in $lists) {
    Set-PnPList -Identity $list -ContentTypesEnabled $true
    Write-Host "Content types enabled for list: $($list.Title)"
}
```




## PowerShell and CSOM

For all lists.

```powershell

function Set-SPOListsContentTypesEnabled {
    param (
        [Parameter(Mandatory=$true, Position=1)]
        [string]$Username,

        [Parameter(Mandatory=$true, Position=2)]
        [string]$AdminPassword,

        [Parameter(Mandatory=$true, Position=3)]
        [string]$Url,

        [Parameter(Mandatory=$true, Position=4)]
        [bool]$ContentTypesEnabled
    )

    # Convert plain text password to SecureString
    $password = ConvertTo-SecureString -string $AdminPassword -AsPlainText -Force

    # Create the ClientContext object
    $ctx = New-Object Microsoft.SharePoint.Client.ClientContext($Url)

    # Set the SharePoint Online credentials
    $ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $password)

    # Execute the query to authenticate
    $ctx.ExecuteQuery()

    # Get all the lists in the site
    $Lists = $ctx.Web.Lists

    # Load the lists
    $ctx.Load($Lists)

    # Execute the query to fetch the lists
    $ctx.ExecuteQuery()

    # Loop through all lists and enable or disable content types
    Foreach ($ll in $Lists) {
        # Set the ContentTypesEnabled property
        $ll.ContentTypesEnabled = $ContentTypesEnabled
        $ll.Update()

        try {
            # Execute the query to update the list
            $ctx.ExecuteQuery()
            Write-Host "$($ll.Title) - Done" -ForegroundColor Green
        }
        catch [Net.WebException] {
            Write-Host "Failed: $($_.Exception.ToString())" -ForegroundColor Red
        }
    }
}

# Add the required SharePoint CSOM DLLs (Please verify the paths)
Add-Type -Path "c:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "c:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

# Define the credentials and site URL
$Username = "trial@trialtrial123.onmicrosoft.com"
$AdminPassword = "Pass"
$Url = "https://trialtrial123.sharepoint.com/sites/teamsitewithlists"
$ContentTypesEnabled = $true

# Call the function to enable content types for all lists
Set-SPOListsContentTypesEnabled -Username $Username -AdminPassword $AdminPassword -Url $Url -ContentTypesEnabled $ContentTypesEnabled
```

### Key Points:

- **ContentTypesEnabled**:
    - Set this parameter to `$true` to enable content types or `$false` to disable them for all lists in the site.

- **$ctx.ExecuteQuery()**: 
    - This is used to execute all changes made to the context, which includes setting the `ContentTypesEnabled` property for each list.

- **Error Handling**: 
    - If the script encounters an issue when trying to update a list, it will catch the exception and print a "Failed" message.

- **Adding CSOM DLLs**: 
    - The script includes paths to the SharePoint CSOM DLLs, which are required for interacting with SharePoint Online using the Client-Side Object Model (CSOM).

### Use Cases:

- **Enable Content Types for all Lists**: 
    - You might use this script when you need to standardize content types across multiple lists in a SharePoint site.

- **Disable Content Types**: 
    - You can run the script with `$ContentTypesEnabled = $false` to disable content type management if required.





