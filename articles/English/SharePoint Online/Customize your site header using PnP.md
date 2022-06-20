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
There are 2 PnP cmdlets which you can use to set the size of your SharePoint Online site header: [**Set-PnPWeb**] and [**Set-PnPWeb**].

Set-PnPWeb -HeaderLayout

Set-PnPWebHeader -HeaderLayout

Accepted values are: None, Standard, Compact, Minimal, Extended


## Set Header Color
There are 2 PnP cmdlets which you can use to set the color of your SharePoint Online site header: [**Set-PnPWeb**] and [**Set-PnPWeb**].

Set-PnPWeb -HeaderEmphasis

Set-PnPWebHeader -HeaderEmphasis

Accepted values: None, Neutral, Soft, Strong


They refer to the colors defined in your theme palette


## Remove Site Title


Set-PnPWeb -HideTitleInHeader

Set-PnPWebHeader -HideTitleInHeader


# Set Header Background Image


[-HeaderBackgroundImageUrl <string>] [-HeaderBackgroundImageFocalX <double>] [-HeaderBackgroundImageFocalY <double>] 



# Set Site Logo