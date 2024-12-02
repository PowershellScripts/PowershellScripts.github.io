---
layout: page
title: 'Create SharePoint dashboard'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-11-30'
---


# Dashboard

A dashboard in SharePoint is a page primarily designed for mobile use, providing users with quick access to essential tools and resources. It typically includes quick links, task-oriented apps, and widgets that allow users to perform frequent actions with minimal effort. SharePoint dashboards are designed to reduce scrolling and navigation, ensuring efficiency for users who need to stay productive while on the go. They can also be customized to display key metrics, news, or workflows tailored to team or organizational needs, integrating seamlessly with other Microsoft 365 applications like Teams, Planner, or Power BI.


<img src="/articles/img/dashboard.PNG" width="600"><br/>


<br/>

# Create a Dashboard

PnP Powershell will give you a lovely Dashboard. Use `Set-PnPPage` and choose Dashboard layout to create a dashboard.


```powershell

Connect-PnPOnline -Url "https://acco967.sharepoint.com/sites/EWZ" -ClientId "74180a34-0000-4642-0000-00009dec9a15" -Interactive

Add-PnPPage -Name "test-dashboard"

Set-PnPPage -Identity "test-dashboard" -LayoutType Dashboard
```



<br/>

# Available Apps

Many apps are available for the SharePoint Dashboard. Approvals, Teams App, Viva Learning, among others:


<img src="/articles/img/dashboard2.PNG" width="600"><br/>



You can also add custom ones:

<img src="/articles/img/dashboard3.PNG" width="600"><br/>



Or create your own using Card Designer:


<img src="/articles/img/dashboard4.PNG" width="600"><br/>




<!-- Default Statcounter code for SPO Dashboard
https://powershellscripts.github.io/articles/en/spo/createdashboard
-->
<script type="text/javascript">
var sc_project=13066191; 
var sc_invisible=1; 
var sc_security="8ab4b9ff"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/13066191/0/8ab4b9ff/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->