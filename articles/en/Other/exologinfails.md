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


Check how many you have. If it's more than one, it is likely to cause an issue. Unless you have a very good reason to keep multiple versions, it's best to have only **one**.

```powershell
get-module -ListAvailable | where {$_.Name -match "Exchange"}


Uninstall all your modules. It will allow for a clean start.

Uninstall-Module -Name ExchangeOnlineManagement -force

Make sure all are uninstalled.

get-module -ListAvailable | where {$_.Name -match "Exchange"}


Install the newest version. Check here to see the current version. Optionally, you can also skip the RequiredVersion parameter.

Install-Module -Name ExchangeOnlineManagement -RequiredVersion 3.6.0

import-module -Name ExchangeOnlineManagement -MinimumVersion 3.0.0 -force