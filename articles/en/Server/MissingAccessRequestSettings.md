---
layout: page
title: 'SharePoint 2013/2016/2019/Online: Missing Access Request Settings'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-03-23'
---

<sup>The following article describes two possible ways to solve missing Access Request Settings from Site Settings >> Site Permissions. <br/><br/>
The solution applies to: SharePoint Server 2013, SharePoint Server 2016, SharePoint Server 2019, SharePoint Online</sup>

 <br/><br/>
<h1>Missing Outgoing Email Settings</h1>
<sup>(applies only to SharePoint Server)</sup>


When you navigate to the Site Settings > Site permissions and see the following view:

 <img src="/articles/images/mes1.png" width="400" alt="SharePoint site permissions" >

Verify Outgoing Email Settings in Central Administration:

<img src="/articles/images/mes2.png" width="400" alt="SharePoint site permissions" >

<img src="/articles/images/mes3.png" width="400" alt="SharePoint site permissions" >

Check out the following articles for possible configurations:

[Configure outgoing email for a SharePoint 2013 farm](https://technet.microsoft.com/en-us/library/cc263462.aspx)

[SharePoint 2016 Outgoing Email Configuration settings](https://social.technet.microsoft.com/wiki/contents/articles/34167.sharepoint-2016-outgoing-email-configuration-settings.aspx)

 <br/><br/>
<h1>Inherited Permissions</h1>
<sup>(applies to both SharePoint Server and SharePoint Online)</sup>

The Access Request Setting is absent when the subsite inherits permissions from its parent:

<img src="/articles/images/mes4.png" width="400" alt="SharePoint site permissions" >

Stop inheriting permissions and the setting becomes available:

<img src="/articles/images/mes5.png" width="400" alt="SharePoint site permissions" >


Results:

<img src="/articles/images/mes6.png" width="400" alt="SharePoint site permissions" >



<!-- Default Statcounter code for
ServerMissingAccessRequests
https://powershellscripts.github.io/articles/en/Server/MissingAccessRequestSettings/
-->
<script type="text/javascript">
var sc_project=13025471; 
var sc_invisible=1; 
var sc_security="10c467a7"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13025471/0/10c467a7/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->