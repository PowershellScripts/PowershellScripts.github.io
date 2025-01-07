---
layout: page
title: 'Microsoft 365: Run As [Product] Admin'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-11-16'
---

This article describes how to use run as admin option in Microsoft 365 admin center.

<h1> Run As Admin </h1>

Managing roles and permissions is a critical part of administering Microsoft 365 environments. To ensure roles are assigned and configured correctly, the "Run As" feature in the Microsoft 365 admin center allows administrators to simulate and test various admin roles. This powerful functionality helps identify gaps in permissions, validate access levels, and streamline role-based configurations without affecting the live environment. Using Run As feature for admin accounts, you can simulate their access and test the permissions before assigning them.

Run as Admin allows you to test the permissions associated with every Microsoft 365 admin role.

<br/><br/><br/>

Navigate to Microsoft 365 admin center and select **Role Assignments** tab. 

<img src="/articles/img/runasadmin.png" >


You will see a list of admin roles, such as AI Administrator, Knowledge Manager, Global Reader, SharePoint Administrator, Viva Pulse Administrator or Virtual Visits Administrator :

<br/>

<img src="/articles/img/simulateadmin.PNG" >


<br/><br/><br/>

Because there are currently over 100 admin roles in Microsoft 365 at the time of writing this, you can easily sort them by Category :

<br/>

<img src="/articles/img/simulateadmin2.PNG" >

<br/><br/><br/>

The admin roles can be also filtered or searched by name:

<br/>



<img src="/articles/img/simulateadmin3.PNG" >


<br/><br/><br/>

After selecting the Microsoft 365 admin role you want to test, such as SharePoint Admin, Exchange Admin or many others, click on **Run As**. 

<br/>

<img src="/articles/img/compareroles5.PNG" >


<br/><br/><br/>

This will open a new browser tab. In this tab, you can navigate your Microsoft 365 tenant as the selected admin role. For example as a Security Admin. Keep in mind that any changes you make to tenant settings during the test will remain. They will NOT be reset after you have finished testing the admin role. Make sure to proceed with caution while testing.

<br/>

<img src="/articles/img/compareroles6.PNG" >

<br/><br/><br/>

<h1> Summary </h1> 

A short video showing how to use **Run As** option.

<video src="/articles/vid/RoleRunAs.mp4"  controls></video>

 
<h1>FAQ</h1>

The functionality can help you answer the following questions:

* Can Billing Administrator buy new licenses?

* Does a Compliance Administrator have access to view user audit logs?
* Can a Security Administrator reset user passwords?
* What permissions does a Global Reader have compared to a Global Administrator?
* Can an Exchange Administrator create shared mailboxes?
* Does a Teams Administrator have the ability to manage user policies for external collaboration?
* Can a User Administrator assign roles to other users?
* What level of access does a SharePoint Administrator have to site collections?
* Does a Dynamics 365 Administrator have the ability to configure new instances?
* Can a Power Platform Administrator view Power BI dashboards?


<!-- Default Statcounter code for runasadmin
https://powershellscripts.github.io/articles/en/InformationProtection/simulateadmin/
-->
<script type="text/javascript">
var sc_project=13062629; 
var sc_invisible=1; 
var sc_security="4f7c59cb"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13062629/0/4f7c59cb/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->