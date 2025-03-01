---
layout: page
title: 'Audit sensitivity labels with Powershell'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2022-08-17'
---



## Sensitivity labels

Sensitivity labels are part of Microsoft Information Protection solution. Sensitivity labels classify and protect your organization's data, by applying appropriate permissions and restrictions on the classified content. As opposed to retention labels, which are published to locations such as all Exchange mailboxes, sensitivity labels are published to users or groups. That means that everywhere where the labels are supported, your users will be able to use them. Apps that support sensitivity labels will display them to the users and groups they were published to. 
The sensitivity labels will show as already applied labels, if they are being applied automatically; or as labels that can be applied, if the Compliance Administrator decided that they can be applied by users.



## Prerequisites

Install Exchange Online module and Connect to Security & Compliance Center PowerShell.
```powershell
Install-Module -Name ExchangeOnlineManagement -RequiredVersion 2.0.5
Connect-IPPSSession -UserPrincipalName User@contoso.com
```

## Verify existing labels 
Use **Get-Label** cmdlet to see the available labels in your environment, their scopes and priorities.

```powershell
Get-Label
```
 <img src="/articles/images/sens30.PNG" width="400">



## Get All Details on a Label

```powershell
Get-Label -Identity keyword-label | fl
```
<img src="/articles/images/sens31.PNG" width="400">


## Get All Label Actions
If you use **-IncludeDetailedLabelActions** parameter, you will see the particular actions assigned to each label.
<br/>
<img src="/articles/images/sens32.PNG" width="400">

## Which Label Causes Trouble
If you are unsure which label is applying specific policy, you can check which labels perform which actions.

```powershell
Get-Label -IncludeDetailedLabelActions $true | select Applywatermarkingtext, displayname
```
<img src="/articles/images/sens34.PNG" width="400">


```powershell
Get-Label -IncludeDetailedLabelActions $true | select EncryptionEnabled, displayname
```
<img src="/articles/images/sens35.PNG" width="400">

## Find auto-labeling labels
Each label has a property called "capabilities". Using it, you can discover which labels are automatically applied to the content.

```powershell
Get-Label | select Capabilities, DisplayName
```

## Audit Matrix for Label Actions
Audit matrix allows you to see the entire overview of labels, their actions, capabilities and settings at a given point of time. It is a simple Excel file, but may be useful if your organization needs to store the settings for compliance reasons. 

<img src="/articles/images/sens36.PNG" width="400">

<img src="/articles/images/sens37.PNG" width="400">


## See Also
[M365 Information protection: Understanding Sensitivity labels vs sensitive information types](https://social.technet.microsoft.com/wiki/contents/articles/54457.m365-information-protection-understanding-sensitivity-labels-vs-sensitive-information-types.aspx)

[Sensitivity labels: Enable labels for groups and sites](https://social.technet.microsoft.com/wiki/contents/articles/54499.sensitivity-labels-enable-labels-for-groups-and-sites.aspx)





<!-- Default Statcounter code for Audit sensitivity labels
with PS
https://powershellscripts.github.io/articles/en/InformationProtection/Audit%20your%20sensitivit
-->
<script type="text/javascript">
var sc_project=12764960; 
var sc_invisible=0; 
var sc_security="9671f253"; 
var sc_client_storage="disabled"; 
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
