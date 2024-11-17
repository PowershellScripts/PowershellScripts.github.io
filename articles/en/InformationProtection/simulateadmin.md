---
layout: page
title: 'Microsoft 365: simulating admin roles'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-11-16'
---


# Run As Admin

Managing roles and permissions is a critical part of administering Microsoft 365 environments. To ensure roles are assigned and configured correctly, the "Run As" feature in the Microsoft 365 admin center allows administrators to simulate and test various admin roles. This powerful functionality helps identify gaps in permissions, validate access levels, and streamline role-based configurations without affecting the live environment. Using Run As feature for admin accounts, you can simulate their access and test the permissions before assigning them.

<br/><br/><br/>

Navigate to Microsoft 365 admin center and select **Role Assignments** tab. You will see a list of admin roles, such as AI Administrator, Knowledge Manager, Global Reader, SharePoint Administrator, Viva Pulse Administrator or Virtual Visits Administrator

<br/>

<img src="/articles/img/simulateadmin.PNG" >


<br/><br/><br/>

Because there are currently ??? admin roles at the time of writing this, you can easily sort them by category

<br/>

<img src="/articles/img/simulateadmin2.PNG" >

<br/><br/><br/>

Filter or search by name:

<br/>



<img src="/articles/img/simulateadmin3.PNG" >


<br/><br/><br/>

After selecting the admin role you want to test, click on **Run As**. This will open a new browser tab. In this tab, you can navigate your Microsoft 365 tenant as the selected admin role. Keep in mind that any changes you make to tenant settings during the test are permanent. Make sure to proceed with caution while testing.

<img src="/articles/img/compareroles6.PNG" >

<br/><br/><br/>

# Summary 

A short video showing the **Run As** option

<video src="/articles/vid/RoleRunAs.mp4"  controls></video>

<img src="/articles/img/compareroles5.PNG" >