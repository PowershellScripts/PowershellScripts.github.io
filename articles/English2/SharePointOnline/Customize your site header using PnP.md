---
layout: page
title: 'Customize your site header using PnP'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2022-06-18'
---



## Introduction

Using PnP cmdlets you can do amazing stuff to customize your SharePoint Online site. In this article we will focus on customizing SharePoint site header, at the top of the SharePoint site. If you haven't installed the module yet, visit the [PnP PowerShell Site](https://pnp.github.io/powershell/index.html) for detailed instructions.



## Set Header Size
There are 2 PnP cmdlets which you can use to set the size of your SharePoint Online site header: [**Set-PnPWeb**](https://pnp.github.io/powershell/cmdlets/Set-PnPWeb.html) and [**Set-PnPWebHeader**](https://pnp.github.io/powershell/cmdlets/Set-PnPWebHeader.html).

```powershell
Set-PnPWeb -HeaderLayout Standard
```
```powershell
Set-PnPWebHeader -HeaderLayout Compact
```
Accepted values are: None, Standard, Compact, Minimal, Extended


| None     | Compact | Extended | Minimal |
| <br/><img src="/articles/images/header5.png" width="200"><br/> | <br/><img src="/articles/images/header6.PNG" width="200"><br/> | <br/><img src="/articles/images/header7.PNG" width="200"><br/> |<br/><img src="/articles/images/header8.PNG" width="200"><br/> |

<br/>

## Set Header Color
There are 2 PnP cmdlets which you can use to set the emphasis of your SharePoint Online site header which is reflected in its color: [**Set-PnPWeb**](https://pnp.github.io/powershell/cmdlets/Set-PnPWeb.html) and [**Set-PnPWebHeader**](https://pnp.github.io/powershell/cmdlets/Set-PnPWebHeader.html).

```powershell
Set-PnPWeb -HeaderEmphasis Soft
```
```powershell
Set-PnPWebHeader -HeaderEmphasis Strong
```
Accepted values: None, Neutral, Soft, Strong

| None     | Neutral| Soft| Strong |
| <br/><img src="/articles/images/header9.png" width="200"><br/> | <br/><img src="/articles/images/header10neu.PNG" width="200"><br/> | <br/><img src="/articles/images/header11.PNG" width="200"><br/> |<br/><img src="/articles/images/header12.PNG" width="200"><br/> |



The exact colors are defined in your theme palette. For more details on color schemes available in your SharePoint Online site theme, have a look at [Generate SharePoint theme using Theme Generator tool](/articles/en/SharePointOnline/Generate%20SharePoint%20theme%20using%20Theme%20Generator%20tool/) and [Available Theme Tokens and Their Occurrences](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/use-theme-colors-in-your-customizations#available-theme-tokens-and-their-occurrences).

<br/>

## Remove Site Title
There are 2 PnP cmdlets that can make site title invisible on your SharePoint site.
```powershell
Set-PnPWeb -HideTitleInHeader
```
```powershell
Set-PnPWebHeader -HideTitleInHeader
```
<br/>

## Set Site Logo
Using [**Set-PnPWeb**](https://pnp.github.io/powershell/cmdlets/Set-PnPWeb.html) or [**Set-PnPWebHeader**](https://pnp.github.io/powershell/cmdlets/Set-PnPWebHeader.html) you can set the image of your site logo.

```powershell
Set-PnPWeb -SiteLogoUrl /sites/floow1/SiteAssets/sitelogo.jpg
```
```powershell
Set-PnPWebHeader -SiteLogoUrl /sites/floow1/SiteAssets/sitelogo.jpg
```

You can also set the position of your SharePoint Online site logo. <strong>Important!</strong> Works only with Extended Layout.
```powershell
Set-PnPWebHeader -LogoAlignment Middle
```
<img src="/articles/images/header15.png" width="400"><br/>
<br/>

```powershell
Set-PnPWebHeader -LogoAlignment Right
```
<img src="/articles/images/header14.png" width="400"><br/>
<br/><br/>

## Set Site Header Background Image
```powershell
Set-PnPWebHeader -HeaderBackgroundImageUrl /sites/floow1/SiteAssets/sitelogo.jpg
```
<img src="/articles/images/header16.png" width="400"><br/>


### Set Focal Point of the Header Background Image
These cmdlets work only with Extended Header Layout. They set the position of the focal point (the yellow circle that you can see in the User Interface):

<img src="/articles/images/header17.PNG" width="200"><br/>

```powershell
Set-PnPWebHeader -HeaderBackgroundImageFocalX 200
Set-PnPWebHeader -HeaderBackgroundImageFocalY 2
```

Site Header Background Image after the update:

<img src="/articles/images/header18.png" width="400"><br/>


<br/><br/>

# Multiple sites
Usually it doesn't make sense to use PowerShell for changing the settings only for one SharePoint site. UI provides similar options and is more accessible for non-developer users. However, for introducing unified settings for thousand sites, nothing matches good ol' Powershell.



The following sample sets the site header and logo on selected SharePoint Online sites, provided through CSV. Make sure one of the columns/properties in your CSV file is SiteUrl.
```powershell
$sites = Import-CSV C:\yourCSVPath.csv
$credentials = Get-Credential

foreach($site in $sites){

    # Connect to the current site
    Connect-PnPOnline -Url $Site.SiteUrl -Credentials $Credentials

    # Set the header image. Organization asset library could be a good place for storing it.
    Set-PnPWebHeader -HeaderBackgroundImageUrl "/sites/OrgAssets/Logos/sitelogo.jpg"
    Set-PnPWebHeader -HeaderBackgroundImageFocalX 200
    Set-PnPWebHeader -HeaderBackgroundImageFocalY 2

    # Set the site logo and logo alignment
    Set-PnPWebHeader -SiteLogoUrl /sites/floow1/SiteAssets/sitelogo.jpg
    Set-PnPWebHeader -LogoAlignment Middle

    # Just a precaution, not necessary, because Connect-PnPOnline disconnects the previous connection
    Disconnect-PnPOnline
}
```


<!-- Default Statcounter code for Customize your site header
using
https://powershellscripts.github.io/articles/en/SharePointOnline/Customize%20your%20site%20head
-->
<script type="text/javascript">
var sc_project=12941101; 
var sc_invisible=1; 
var sc_security="af700d2d"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/12941101/0/af700d2d/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
