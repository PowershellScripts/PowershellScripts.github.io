---
layout: page
title: 'SharePoint Online site classification'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2022-06-16'
---

## First: Enable the capability
Site classification has to be enabled at the Azure AD level.
After enabling the site classification capability at the Azure AD level, you see an additional field *How sensitive is your data?* while creating new sites. Site classification allows you to define the sensitivity of your data on site level and group your sites based on the sensitivity of the information they contain.

You can enable the capability using [**Enable-PnPSiteClassification**](https://pnp.github.io/powershell/cmdlets/Enable-PnPSiteClassification.html) cmdlet.

If you are interested in more details, you can find them in a more dev-oriented [Implement Sharepoint site classification solution Guidance](https://docs.microsoft.com/en-us/sharepoint/dev/solution-guidance/implement-a-sharepoint-site-classification-solution) from Microsoft.

If you are getting any errors at this point, you may want to resort to AzureADPreview Powershell module and check your AD settings for any existing templates, using [**Get-AzureADDirectorySetting**](https://docs.microsoft.com/en-us/powershell/module/azuread/get-azureaddirectorysetting?view=azureadps-2.0-preview) cmdlet.

### Troubleshoot with AzureADPreview module
```
# Install the Azure AD Preview Module for PowerShell
Install-Module AzureADPreview

# Connect to Azure AD
Connect-AzureAD

# Get existing settings
Get-AzureADDirectorySetting
```

<img src="/articles/images/classification8.PNG" width="400">


If you see any existing settings like in the screenshot above, probably someone else in your organization already set them.


## Verify the existing site classification

### Using PnP
Use **Get-PnPSiteClassification** cmdlet to retrieve the existing site classification settings. As a result, you receive the values of UsageGuidelinesUrl, Classifications, and DefaultClassification. DefaultClassification value has to be also in the list of values you see under Classifications.

```
Get-PnPSiteClassification
```
<img src="/articles/images/classification3.PNG" width="400">

 
### Using AzureADPreview
Use [**Get-AzureADDirectorySetting**](https://docs.microsoft.com/en-us/powershell/module/azuread/get-azureaddirectorysetting?view=azureadps-2.0-preview) cmdlet to retrieve the existing site classification settings. As a result, you receive the values of UsageGuidelinesUrl, Classifications, and DefaultClassification.



### Results
In the User Interface you can see these values when you create a new site:

 <img src="/articles/images/classification2.PNG" width="400">




## Update Site Classification
Use **Update-PnPSiteClassification** cmdlet to set the available classifications or usage guidelines url.

### Update Site classification options
```
Update-PnPSiteClassification -Classifications "HBI", "CRI", "LBI"
```
 <img src="/articles/images/classification4.PNG" width="400"> 


### Update usage guidelines url
```
Update-PnPSiteClassification -UsageGuidelinesUrl "https://powershellscripts.github.io/"
```
 <img src="/articles/images/classification6.PNG" width="400">



# See Also

Another solution for classifying your sites - sensitivity labels, 
offering a more holistic approach on entire tenant scale, including also e.g. Exchange Online <br/>
[Protecting your information: the power of labels](https://social.technet.microsoft.com/wiki/contents/articles/54468.protecting-your-information-the-power-of-labels.aspx) <br/>
[Sensitivity labels: Powershell your way around](https://social.technet.microsoft.com/wiki/contents/articles/54497.sensitivity-labels-powershell-your-way-around.aspx) <br/>
[Audit your sensitivity labels with Powershell](https://powershellscripts.github.io/articles/English/InformationProtection/Audit%20your%20sensitivity%20labels%20with%20Powershell/)<br/>
<br/>

Absolutely necessary one-time step to enable sensitivity labels on site level:<br/>
[Sensitivity labels: Enable labels for groups and sites](https://social.technet.microsoft.com/wiki/contents/articles/54499.sensitivity-labels-enable-labels-for-groups-and-sites.aspx)