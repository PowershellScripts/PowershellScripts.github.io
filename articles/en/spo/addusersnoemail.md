---
layout: page
title: 'Add users to SharePoint without sending them an email'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-02-15'
---

Usually, if you invite users to your site, they will receive an email notification. Here is how to avoid it.


# Entire site


To invite a user to your SharePoint Online site without sending an email, first, navigate to your site and open the Site Access panel. 

<img src="/articles/img/inviteusersnoemail.png" width="600" alt="screenshot of SharePoint site"><br/>

Enter the user’s name or email address. 

<img src="/articles/img/inviteusersnoemail2.png" width="600" alt="screenshot of sharing SharePoint site"><br/>


Select their desired permission level (e.g., Read, Edit). Uncheck or disable the Notify by Email option. 

<img src="/articles/img/inviteusersnoemail3.png" width="600" alt="screenshot of sharing SharePoint site"><br/>


Once done, click Share, and the user will be granted access without receiving an email notification.





# Library

In order to invite your user to library only and not other libraries or lists in your site, you need to to first break permission inheritance.

Usually libraries have the same permissions as the site collection (they inherit permissions). If a library needs special permissions (more or less), it needs to have unique permissions.

* Navigate to library settings. Click on the gear icon in the top right corner and then select Library Settings:


<img src="/articles/img/inviteusersnoemail5.png" width="600" alt="screenshot of SharePoint site"><br/>


* Select **Permissions for this document library**

<img src="/articles/img/inviteusersnoemail6.png" width="600" alt="screenshot of SharePoint site"><br/>


* Select **Stop Inheriting Permissions**

<img src="/articles/img/inviteusersnoemail7.png" width="600" alt="screenshot of SharePoint site"><br/>

You will receive a prompt to confirm your decision. Select **Yes**.

*You are about to create unique permissions for this document library. Changes made to the parent site permissions will no longer affect this document library.*



* Once the library has unique permissions, a new button will appear. Select **Grant Permissions**

<img src="/articles/img/inviteusersnoemail8.png" width="600" alt="screenshot of SharePoint site"><br/>



* Enter user's name or email adress und uncheck **Send an email invitation**


<img src="/articles/img/inviteusersnoemail8.png" width="600" alt="screenshot of SharePoint site"><br/>





<br/><br/>

## Happy sharing !






<!-- Default Statcounter code for disattachplusdownloadable
https://powershellscripts.github.io/articles/en/spo/disableattachments/
-->
<script type="text/javascript">
var sc_project=13087706; 
var sc_invisible=1; 
var sc_security="cd768061"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13087706/0/cd768061/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->