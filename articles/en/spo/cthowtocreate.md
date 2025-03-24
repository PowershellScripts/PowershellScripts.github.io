---
layout: page
title: 'How to create SharePoint content type'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-03-22'
---


A content type can be a form, a document template you want to use to gather information. Behind the scenes, a content type in SharePoint is a reusable collection of metadata (columns), workflows, and behavior settings for items in a list or library. Here is a step-by-step guide to create a content type in SharePoint Online.


# Site


To create a content type that will be available for all lists and libraries in your site collection, navigate to **Site Settings**. Ensure you have appropriate permissions (Site Owner or Admin).
  1. Click on the Settings icon (gear icon) in the top-right corner.
  2. Select **Site information** > View all site settings.


<img src="/articles/img/ctget4.PNG" width="600">


In the site settings select **Content Types**. Click on **Create**.



<img src="/articles/img/ctcreate0.png" width="600">




<img src="/articles/img/ctcreate1.png" width="600">


Once the content type is created, add the columns/properties that you need. 

<img src="/articles/img/ctcreate3.png" width="600">



# Add the Content Type to List or Library

First of all, make sure you have [enabled content type management](https://powershellscripts.github.io/articles/en/spo/enablect/) in your library.

If the content management is enabled, go to your list or library and click on **Add column**. Scroll to the bottom of your column list and select **Add a content type**

<img src="/articles/img/ctcreate2.PNG" width="600">


Select the content type that you created.



# See Also

[Enable content type management](https://powershellscripts.github.io/articles/en/spo/enablect/)

[Add content type using Powershell and CSOM](https://powershellscripts.github.io/articles/en/SharePointOnline/Add%20content%20type/)


[Find content type ID](https://powershellscripts.github.io/articles/en/SharePointOnline/findctid/)