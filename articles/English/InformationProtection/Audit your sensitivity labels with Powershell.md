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


Sensitivity labels

Sensitivity labels are part of Microsoft Information Protection solution. Sensitivity labels classify and protect your organization's data, by applying appropriate permissions and restrictions on the classified content. As opposed to retention labels, which are published to locations such as all Exchange mailboxes, sensitivity labels are published to users or groups. That means that everywhere where the labels are supported, your users will be able to use them. Apps that support sensitivity labels will display them to the users and groups they were published to. 
The sensitivity labels will show as already applied labels, or as labels that they can apply, depending whether the Compliance Administrator decided that they can be applied by users or automatically.



Prerequisites

Make sure you install Exchange Online module Jump and Connect to Security & Compliance Center PowerShell Jump in a Microsoft 365

Install-Module -Name ExchangeOnlineManagement -RequiredVersion 2.0.5
Connect-IPPSSession -UserPrincipalName User@contoso.com



Get All Details on a Label




Get All Label Actions