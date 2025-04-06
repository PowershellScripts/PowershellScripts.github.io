---
layout: page
title: 'ExchangeOnlineManagement fails to connect'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-04-06'
---


ExchangeOnlineManagement fails to connect


Connect-ExchangeOnline -UserPrincipalName
InvalidOperation: You cannot call a method on a null-valued expression.


Check how many you have

get-module -ListAvailable | where {$_.Name -match "Exchange"}

Uninstall-Module -Name ExchangeOnlineManagement -force

get-module -ListAvailable | where {$_.Name -match "Exchange"}


Install-Module -Name ExchangeOnlineManagement -RequiredVersion 3.6.0

import-module -Name ExchangeOnlineManagement -MinimumVersion 3.0.0 -force