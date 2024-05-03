---
layout: page
title: 'Who created most flows?'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2022-06-01'
---


## Introduction

Using PnP you can easily retrieve users who are creating flows and extract data statistics. That allows you to get the most prolific Power Automate creators who created the most flows.
<br/>
<br/>

## Get Flow Properties

First, in order to see who created a flow, retrieve properties of the flow. Remember to use -AsAdmin switch. Otherwise you will get only your own flows.
```
Connect-PnpOnline
$environment = Get-PnPPowerPlatformEnvironment
$Flows = Get-PnPFlow -Environment $environment -AsAdmin | select -expandProperty Properties
```
<img src="/articles/images/flows18.PNG" width="600"> 

One of those properties is the Creator. If you expand it, you obtain ObjectId, which is Azure Active Directory ObjectId.
<img src="/articles/images/flows104.PNG" width="600"> 

Using ```Group-Object``` and ```Sort-Object``` cmdlets, you get the users who created most flows.
```
$props.Creator | Group-Object -Property ObjectId -NoElement  | Sort-Object -Descending
```
 
<img src="/articles/images/flows19.PNG" width="600"> 
<br/>


## Get Azure Active Directory Users

Use Azure Active Directory ObjectId to obtain user's UPN or email address.
```
Connect-MSOLService
Get-MsolUser | where {$_.ObjectId -eq "f655dd56-ffea-45ad-aa45-775e4e0eeb9b"}
```


<br/>
<br/>

## Full Script

```
Connect-PnPOnline
$environment = Get-PnPPowerPlatformEnvironment
$flowprops = Get-PnPFlow -AsAdmin -Environment $environment | select -ExpandProperty Properties
$MostProlific = $flowprops.Creator | Group-Object -Property ObjectId | sort
 
$MostProlific | Foreach-Object {
    $FlowCreator = $_ ; 
    $user = Get-MsolUser | where {
        $_.ObjectId -eq $FlowCreator.Name
    };
    $user | Add-Member -MemberType NoteProperty -Name NoOfFlow -Value $FlowCreator.Count;
    Write-Host $user.DisplayName $user.NoOfFlow
    $user | Export-CSV -Path yourcsvpath.csv -Append
}
```
<img src="/articles/images/flow21.PNG" width="600">  

<!-- Default Statcounter code for Github - Most Flow
Creators
https://powershellscripts.github.io/articles/en/PowerPlatform/Get%20users%20who%20create%20most
-->
<script type="text/javascript">
var sc_project=12763867; 
var sc_invisible=0; 
var sc_security="3cad39f3"; 
var scJsHost = "https://";
document.write("<sc"+"ript type='text/javascript' src='" +
scJsHost+
"statcounter.com/counter/counter.js'></"+"script>");
</script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/12763867/0/3cad39f3/0/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
