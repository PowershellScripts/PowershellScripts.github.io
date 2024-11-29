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

Set-PnPPage -Identity "MyPage" -HeaderType None



page.PageHeader.LayoutType = PageHeaderLayoutType.ColorBlock;
page.PageHeader.ShowTopicHeader = true;
page.PageHeader.TopicHeader = "I'm a topic header";
page.PageHeader.TextAlignment = PageHeaderTitleAlignment.Center;
page.PageHeader.ShowPublishDate = true;