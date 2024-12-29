---
layout: page
title: 'Customize your page header'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-11-23'
---


# Set page header
Using PnP you can customize your page header



```powershell
Set-PnPPage -Identity "MyPage" -HeaderType None
```

<img src="/articles/img/pageheadernone.PNG" width="600"><br/>

```powershell
Set-PnPPage -Identity "MyPage" -HeaderType Default
```

<img src="/articles/img/pageheaderdefault.PNG" width="600"><br/>



# See Also


[Customize your site header using PnP](https://powershellscripts.github.io/articles/en/SharePointOnline/setsiteheaderPnP/)



<!-- Default Statcounter code for SPO page header
https://powershellscripts.github.io/articles/en/SharePointOnline/custompageheader
-->
<script type="text/javascript">
var sc_project=13066189; 
var sc_invisible=1; 
var sc_security="245461cc"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/13066189/0/245461cc/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->