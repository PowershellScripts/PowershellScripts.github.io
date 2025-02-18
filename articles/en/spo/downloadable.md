---
layout: page
title: 'Create download link to SharePoint file'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-02-09'
---


When you create a link to a file in SharePoint using the standard method, it opens in the browser by default. To make the file downloadable instead, you can customize the link to trigger a download rather than opening the file. This can be done by adjusting the file's URL or using specific download settings in SharePoint.



Navigate to your library and select the file. Click on three dots (...) and select **Download**

<img src="/articles/img/downloadable.png" width="600" alt="screenshot showing a SharePoint library"><br/>

In your browser click on the download icon open the **Download History**


<img src="/articles/img/downloadable2.png" width="600" alt="screenshot showing a SharePoint library"><br/>


Copy the link from your download history:

<img src="/articles/img/downloadable3.png" width="600" alt="screenshot showing downloads history"><br/>


It looks similar to: https://TENANT.sharepoint.com/sites/EWZ/_layouts/15/download.aspx?UniqueId=cf6f2e20%2D8050%2D439e%2Dbf00%2D8827ca8bac19


You can also create the downloadable link on your own by replacing TENANT, SITENAME and UNIQUEID with your values in the following template:

https://TENANT.sharepoint.com/sites/SITENAME/_layouts/15/download.aspx?UniqueId=UNIQUEID

<br/>

### Short video


<iframe width="660" height="400" src="https://www.youtube.com/embed/Ypm0qhYxz90" frameborder="0" allowfullscreen></iframe>



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