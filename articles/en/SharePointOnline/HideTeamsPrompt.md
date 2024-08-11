---
layout: page
title: 'Hide Teams Prompt'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-03-24'
---

<h1>Hide teamify prompt</h1>

In SharePoint, when a site is created without being associated with a Microsoft 365 group or a Microsoft Teams team, users typically encounter a teamify prompt displayed in the bottom left corner of the site interface. This prompt serves as a notification to users that the site does not have the collaborative features and functionalities associated with sites integrated with Teams or Microsoft 365 groups. 

<img src="/articles/images/teamify1.PNG"><br/>

<br/>
<h1>Method 1</h1>

You can remove the teamify prompt using PnP module cmdlet `Set-PnPTeamifyPromptHidden` . 


```powershell
$SiteURL = "https://t345.sharepoint.com/sites/MySite"
 
#Connect to PnP Online
Connect-PnPOnline -URL $SiteURL -Interactive

# Set the teamify prompt to be hidden
Set-PnPTeamifyPromptHidden
```

You can find more on the [Set-PnPTeamifyPromptHidden](https://pnp.github.io/powershell/cmdlets/Set-PnPTeamifyPromptHidden.html) cmdlet 
in the [PnP documentation](https://pnp.github.io).


<br/>
<h1>Method 2</h1>

You can also remove the teamify prompt using PnP cmdlet to set a property bag value. 

```powershell
$SiteURL = "https://t345.sharepoint.com/sites/MySite"
 
#Connect to PnP Online
Connect-PnPOnline -URL $SiteURL -Interactive
 
#Set the property bag to hide the teamify prompt
Set-PnPPropertyBagValue -Key "TeamifyHidden" -Value "True"
```

<br/>
<h1>NoScript error</h1>

If you are receiving <i>Access denied. You do not have permission to perform this action or access this resource. Site might have NoScript enabled, this prevents setting some property bag values</i> make sure you enable the Custom Scripts. Make sure you are aware of all the security risks.


```powershell
  $site = Get-PnPTenantSite -Detailed -Url $SiteURL.URL
  $site.DenyAddAndCustomizePages = 'Disabled'
  $site.Update()
  $site.Context.ExecuteQuery()
```

<br/>
<h1>Results</h1>
After running the short Powershell script, make sure you clear cache and refresh your website. The teamify prompt should be gone.


<!-- Default Statcounter code for Hide Teams Prompt
https://powershellscripts.github.io/articles/en/SharePointOnline/HideTeamsPrompt
-->
<script type="text/javascript">
var sc_project=12981484; 
var sc_invisible=1; 
var sc_security="cf260827"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/12981484/0/cf260827/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
