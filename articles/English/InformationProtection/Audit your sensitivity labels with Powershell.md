---
layout: page
title: 'Audit sensitivity labels with Powershell'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2022-06-17'
---


## Sensitivity labels

Sensitivity labels are part of Microsoft Information Protection solution. Sensitivity labels classify and protect your organization's data, by applying appropriate permissions and restrictions on the classified content. As opposed to retention labels, which are published to locations such as all Exchange mailboxes, sensitivity labels are published to users or groups. That means that everywhere where the labels are supported, your users will be able to use them. Apps that support sensitivity labels will display them to the users and groups they were published to. 
The sensitivity labels will show as already applied labels, or as labels that they can apply, depending whether the Compliance Administrator decided that they can be applied by users or automatically.



## Prerequisites

Install Exchange Online module and Connect to Security & Compliance Center PowerShell.
```
Install-Module -Name ExchangeOnlineManagement -RequiredVersion 2.0.5
Connect-IPPSSession -UserPrincipalName User@contoso.com
```

## Verify existing labels 
Use Get-Label cmdlet to see the available labels in your environment, their scopes and priorities.





Get All Details on a Label




Get All Label Actions



See Also



<!-- Default Statcounter code for Audit sensitivity labels
with PS
https://powershellscripts.github.io/articles/English/InformationProtection/Audit%20your%20sensitivit
-->
<script type="text/javascript">
var sc_project=12764960; 
var sc_invisible=1; 
var sc_security="9671f253"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/12764960/0/9671f253/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
