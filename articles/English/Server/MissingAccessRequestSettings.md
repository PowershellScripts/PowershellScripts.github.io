---
layout: page
title: 'SharePoint 2013/2016/2019/Online: Missing Access Request Settings'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-03-22'
---


The following article describes two possible ways to solve missing Access Request Settings from Site Settings >> Site Permissions.
The solution applies to: SharePoint Server 2013, SharePoint Server 2016, SharePoint Server 2019, SharePoint Online

<h1>Missing Outgoing Email Settings</h1>
(applies only to SharePoint Server)


When you navigate to the Site Settings > Site permissions and see the following view:


Verify Outgoing Email Settings in Central Administration:




Check out the following articles for possible configurations:

[Configure outgoing email for a SharePoint 2013 farm](https://technet.microsoft.com/en-us/library/cc263462.aspx)
[SharePoint 2016 Outgoing Email Configuration settings](https://social.technet.microsoft.com/wiki/contents/articles/34167.sharepoint-2016-outgoing-email-configuration-settings.aspx)

<h1>Inherited Permissions</h1>
(applies also to SharePoint Online)

The Access Request Setting is also missing when the subsite is inheriting permissions from its parent:


Stop inheriting permissions and the setting becomes available:


