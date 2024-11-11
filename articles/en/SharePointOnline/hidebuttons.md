---
layout: page
title: 'SharePoint list view: hide new button'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-11-09'
---


This article will give you a few json formatting examples on how to hide buttons in the command bar of a SharePoint list view, such as "add new item", or "automate". 


# TL;DR;

You can hide one or more buttons in the command bar of your SharePoint list by using the following json. Pick only the keys you want to hide!


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



# Intro

The command bar in SharePoint Online list view has several buttons:



And some extra ones that appear when you select an item:




IMG

A list of command bar buttons with the corresponding key:


# Hide a button in the command bar
If you want to hide e.g. "add new item" button in the command bar of your SharePoint Online list, navigate to 



Examples