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

Using PnP cmdlets you can do amazing stuff to customize your SharePoint Online site. If you haven't installed the module yet, visit the [PnP PowerShell Site](https://pnp.github.io/powershell/index.html) for detailed instructions.



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


## Set Header Color
There are 2 PnP cmdlets which you can use to set the color of your SharePoint Online site header: [**Set-PnPWeb**](https://pnp.github.io/powershell/cmdlets/Set-PnPWeb.html) and [**Set-PnPWebHeader**](https://pnp.github.io/powershell/cmdlets/Set-PnPWebHeader.html).

```powershell
Set-PnPWeb -HeaderEmphasis Soft
```
```powershell
Set-PnPWebHeader -HeaderEmphasis Strong
```
Accepted values: None, Neutral, Soft, Strong

| None     | Neutral| Soft| Strong |
| <br/><img src="/articles/images/header9.png" width="200"><br/> | <br/><img src="/articles/images/header10neu.PNG" width="200"><br/> | <br/><img src="/articles/images/header11.PNG" width="200"><br/> |<br/><img src="/articles/images/header12.PNG" width="200"><br/> |



They refer to the colors defined in your theme palette. For more details on color schemes available in your SharePoint Online site theme, have a look at [Generate SharePoint theme using Theme Generator tool](/articles/English/SharePointOnline/Generate%20SharePoint%20theme%20using%20Theme%20Generator%20tool/)


## Remove Site Title


Set-PnPWeb -HideTitleInHeader

Set-PnPWebHeader -HideTitleInHeader


# Set Header Background Image

Set-PnPWebHeader
[-HeaderBackgroundImageUrl <string>] [-HeaderBackgroundImageFocalX <double>] [-HeaderBackgroundImageFocalY <double>] 



# Set Site Logo