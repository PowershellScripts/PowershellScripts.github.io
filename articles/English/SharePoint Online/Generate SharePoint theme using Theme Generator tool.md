---
layout: page
title: 'Generate SharePoint theme using Theme Generator tool'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2022-06-21'
---



## Introduction

Your organization's SharePoint Online themes are usually a tricky subject. Developed by graphic designers, they need to be implemented by SharePoint Admins. Usually that's two different people. Usually the former has no idea what PowerShell is. The latter doesn't see the difference between mauve and purple. Somehow they have to work together.

# Online Theme Generator tool

I found the Theme Generator tool very helpful in these scenarios. It's a free tool that helps you design your color palette. A single SharePoint Online site design consists of tens of different colors. The Theme Generator explains, where which of these colors is used - links, buttons, etc. It allows you to choose the colors using RGB, #HEX or UI. Then it generates the desired palette theme in a SharePoint Admin-friendly format - Powershell, JSON or ZIP. 


# Step 1 Pick your colors
Navigate to the [online Theme Generator tool](https://fluentuipr.z22.web.core.windows.net/heads/master/theming-designer/index.html)

On the left you can pick the main 3 colors for your design. If you click on the color, you can define them using color picker. On the right you immediately see the effects on a small but good sample site layout. You can see how the chosen colors affect links, buttons, background and mouseover highlights.

<br/>
<img src="/articles/images/header1.png" width="200">
<br/>


If that's not enough for the Graphic Designer (it probably isn't!), scroll down the Theme Generator page for more options. The fabric palette slots allow you to define your new site theme in a greater level of detail. Same as before, click on the little square to set the color for a particular property.

<br/>
<img src="/articles/images/header4.png" width="200">
<br/>

If that's still not the level of detail you need and you have to know exactly where each of the defined colors occur, check out [Available Theme Tokens and Their Occurrences](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/use-theme-colors-in-your-customizations#available-theme-tokens-and-their-occurrences) in official Microsoft Docs to see where in SharePoint the tokens are applied.



# Step 2 Accessibility

There is a variety of rules that ensure that the site is readable and "pleasant to the eye". The rules, recognized accessibility standards like the Web Content Accessibility Guidelines (WCAG), and legislations such as the Americans with Disabilities Act (ADA) ensure great experience for a broad range of users. The quick checker available with the online Theme Generator may help you with that
and show any potential accessibility errors with your color palette.


<br/>
<img src="/articles/images/header3.png" width="200">
<br/>


# Step 3 Generate your theme

Once you are ready with your theme, picked the desired colors and verified accessibility requirements, you can proceed to export it. IN the top right corner click the button **Export theme**. Choose the right format and export it.



# Step 4 Deploy 
There are quite a few ways to deploy your SharePoint Online site theme. You can do it using [Javascript and REST](https://docs.microsoft.com/en-us/sharepoint/dev/declarative-customization/site-theming/sharepoint-site-theming-rest-api), C#, SharePoint Online Management Shell or PnP cmdlets. Below, you can find two examples using PowerShell.

### Deploy site theme using PnP

```powershell
$themepalette = @{
"themePrimary" = "#007454";
"themeLighterAlt" = "#000503";
"themeLighter" = "#00120d";
"themeLight" = "#002219";
"themeTertiary" = "#004531";
"themeSecondary" = "#006548";
"themeDarkAlt" = "#0d8160";
"themeDark" = "#249474";
"themeDarker" = "#4eb094";
"neutralLighterAlt" = "#1b0202";
"neutralLighter" = "#1b0202";
"neutralLight" = "#1a0202";
"neutralQuaternaryAlt" = "#180202";
"neutralQuaternary" = "#170202";
"neutralTertiaryAlt" = "#160202";
"neutralTertiary" = "#c8c8c8";
"neutralSecondary" = "#d0d0d0";
"neutralSecondaryAlt" = "#d0d0d0";
"neutralPrimaryAlt" = "#3b22ab";
"neutralPrimary" = "#ffffff";
"neutralDark" = "#74ba65";
"black" = "#f8f8f8";
"white" = "#1c0303";
}
Add-PnPTenantTheme -Identity "ArletasTheme" -Palette $themepalette -IsInverted $false
```


### Deploy site theme using SharePoint Online Management Shell

```powershell
$themepalette = @{
"themePrimary" = "#007454";
"themeLighterAlt" = "#000503";
"themeLighter" = "#00120d";
"themeLight" = "#002219";
"themeTertiary" = "#004531";
"themeSecondary" = "#006548";
"themeDarkAlt" = "#0d8160";
"themeDark" = "#249474";
"themeDarker" = "#4eb094";
"neutralLighterAlt" = "#1b0202";
"neutralLighter" = "#1b0202";
"neutralLight" = "#1a0202";
"neutralQuaternaryAlt" = "#180202";
"neutralQuaternary" = "#170202";
"neutralTertiaryAlt" = "#160202";
"neutralTertiary" = "#c8c8c8";
"neutralSecondary" = "#d0d0d0";
"neutralSecondaryAlt" = "#d0d0d0";
"neutralPrimaryAlt" = "#3b22ab";
"neutralPrimary" = "#ffffff";
"neutralDark" = "#74ba65";
"black" = "#f8f8f8";
"white" = "#1c0303";
}

Add-SPOTheme -Identity "ArletasThemeFromSPOShell" -Palette $themepalette -IsInverted $false
```


# Apply


Set-SPOWebTheme -Theme "Custom cyan" -Web https://contoso.sharepoint.com/sites/Contoso1

Set-PnPWebTheme
Set-PnPWebTheme -Theme MyTheme
Set-PnPWebTheme -Theme "MyCompanyTheme" -WebUrl https://contoso.sharepoint.com/sites/MyWeb
Get-PnPTheme

# See Also

