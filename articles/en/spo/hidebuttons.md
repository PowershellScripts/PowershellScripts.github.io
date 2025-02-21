---
layout: page
title: 'SharePoint list view: hide new button'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-11-09 Updated: 2025-02-16'
---


This article will give you a few JSON formatting examples on how to hide buttons in the command bar of a SharePoint list view, such as "add new item", or "automate". 


# TL;DR;

You can hide one or more buttons in the command bar of your SharePoint list by using the following JSON. Pick only the keys you want to hide!

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/row-formatting.schema.json",
  "commandBarProps": {
    "commands": [
      {
        "key": "download",
        "hide": false
      },
      {
        "key": "share",
        "hide": true
      },
      {
        "key": "sync",
        "hide": false
      },
      {
        "key": "export",
        "hide": true
      },
      {
        "key": "automate",
        "hide": true
      },
      {
        "key": "integrate",
        "hide": true
      },
      {
        "key": "undo",
        "hide": true
      },
      {
        "key": "new",
        "hide": false
      },
      {
        "key": "editInGridView",
        "hide": true
      },
      {
        "key": "manageForms",
        "hide": true
      },
      {
        "key": "alertMe",
        "hide": true
      },
      {
        "key": "manageAlert",
        "hide": true
      },
      {
        "key": "edit",
        "hide": true
      },
      {
        "key": "copyLink",
        "hide": true
      },
      {
        "key": "comment",
        "hide": true
      },
      {
        "key": "delete",
        "hide": true
      },
      {
        "key": "versionHistory",
        "hide": true
      }
    ]
  }
}

```

<br/>


# Intro

The command bar in the SharePoint Online list view has several buttons:

* Add new item
* Edit in grid view
* Undo
* Share
* Export
* Forms
* Automate
* Integrate
* Alert me
* Manage my alerts


And some extra buttons that appear in the command bar when you select an item:

* Edit
* Share
* Copy link
* Comment
* Delete
* Version history


<img src="/articles/img/hidebuttons7.png" >

<img src="/articles/images/hidebuttons.png" >

<br/>

# Hide a button in the command bar

If you want to hide e.g. "add new item" button in the command bar of your SharePoint Online list, navigate to the view name on the right (e.g. "All Items"), and click "Format current view"

<img src="/articles/img/hidebuttons3.png" >

 At the bottom there is an Advanced Mode button. You may need to scroll. Click on the Advanced Mode:

<img src="/articles/img/hidebuttons2.png" >

and enter the JSON code with your selected keys.  

<img src="/articles/images/hidebuttons4.png" >



Mind you, the buttons are only hidden. The functionalities still exist. Hiding the keys from the SharePoint list does not affect users' permissions. If they find a creative way (e.g. via Graph API) to add a new item, or create a flow starting from Power Platform entry point - they can still do it.

<img src="/articles/images/hidebuttons.png" >

<br/><br/>

### A list of command bar buttons with the corresponding key

The keys have very intuitive names. Most of them are called exactly by their display name (the name you see in the user interface). There are only a few exceptions. Here you can find the list of command bar buttons with the corresponding key:

| Name | Key |
| -------- | ------- |
| Add new item | new |
| Edit in grid view | editInGridView |
| Undo | undo |
| Share | share |
| Export | export |
| Forms | manageForms |
| Automate| automate |
| Alert me | alertMe |
| Manage my alerts | manageAlert |
| Edit | edit |
| Copy link | copyLink |
| Comment | comment |
| Delete | delete |
| Version history | versionHistory |


<br/>

Full list available here: [Command bar customization syntax reference](https://learn.microsoft.com/en-us/sharepoint/dev/declarative-customization/view-commandbar-formatting) 

# Examples

### Example 1

Hide the alerts functionality - "Alert me" and "Manage my alerts":

<img src="/articles/images/hidebuttons5.png" >


```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/row-formatting.schema.json",
  "commandBarProps": {
    "commands": [
      {
        "key": "alertMe",
        "hide": true
      },
      {
        "key": "manageAlert",
        "hide": true
      }
    ]
  }
}

```

### Example 2

Hide **Share**, **Export**, and **Automate** buttons from the command bar of the SharePoint list:

<img src="/articles/images/hidebuttons4.png" >

### Example 3

Hide **Forms** button from the command bar of the SharePoint list:

<img src="/articles/images/hidebuttons6.png" >






<!-- Default Statcounter code for hide buttons
https://powershellscripts.github.io/articles/en/spo/hidebuttons/
-->
<script type="text/javascript">
var sc_project=13078073; 
var sc_invisible=1; 
var sc_security="0820e95d"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13078073/0/0820e95d/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->