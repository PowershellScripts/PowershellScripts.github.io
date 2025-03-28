---
layout: page
title: 'How to create SharePoint content type'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-03-22'
---


A content type can be a form, a document template you want to use to gather information. Behind the scenes, a content type in SharePoint is a reusable collection of metadata (columns), workflows, and behavior settings for items in a list or library. Here is a step-by-step guide to create a content type in SharePoint Online.


# Create


To create a content type that will be available for all lists and libraries in your site collection, navigate to **Site Settings**. Ensure you have appropriate permissions (Site Owner or Admin).
  1. Click on the Settings icon (gear icon) in the top-right corner.
  2. Select **Site information** > View all site settings.


<img src="/articles/img/ctget4.png" width="600">


In the site settings select **Content Types**. Click on **Create**.



<img src="/articles/img/ctcreate0.png" width="600">


A side menu will open. Enter name, description, category and parent content type. **Parent content type** is especially important because it decides if your content type becomes available on your list or library. For example, if you choose a Document as your parent content type, your custom content type can be added to document libraries but not lists. If you choose an Item as your parent content type, your custom content type will be offered in lists.


<img src="/articles/img/ctcreate1.png" width="800">


Once the content type is created, add the columns/properties that you need. 

<img src="/articles/img/ctcreate3.png" width="800">


Now that your content type is ready, you can add it to your list or library and start using it.


<br/>

# Add the Content Type to List or Library

First of all, make sure you have [enabled content type management](https://powershellscripts.github.io/articles/en/spo/enablect/) in your library.

If the content management is enabled, go to your list or library and click on **Add column**. Scroll to the bottom of your column list and select **Add a content type**

<img src="/articles/img/ctcreate2.png" width="600">


Select the content type that you created.

<img src="/articles/img/ctcreate4.png" width="600">



<br/><br/>

# See Also

[Enable content type management](https://powershellscripts.github.io/articles/en/spo/enablect/)

[Add content type using Powershell and CSOM](https://powershellscripts.github.io/articles/en/SharePointOnline/Add%20content%20type/)


[Find content type ID](https://powershellscripts.github.io/articles/en/SharePointOnline/findctid/)






<!-- Default Statcounter code for SPO enable ct
https://powershellscripts.github.io/articles/en/spo/enablect/
-->
<script type="text/javascript">
var sc_project=13073435; 
var sc_invisible=1; 
var sc_security="a5c7940a"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13073435/0/a5c7940a/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->