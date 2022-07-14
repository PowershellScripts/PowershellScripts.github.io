---
layout: page
title: 'Who created most flows?'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2022-06-01'
---

Using PnP PowerShell you can easily see all flows created by other users as well as yourself.

First, in order to see who created a flow, retrieve properties of the flow. Remember to use -AsAdmin switch. Otherwise you will get only your own flows.
```
Connect-PnpOnline
$environment = Get-PnPPowerPlatformEnvironment
$Flows = Get-PnPFlow -Environment $environment -AsAdmin | select -expandProperty Properties
```
<img src="/articles/images/flows18.PNG" width="600"> 
