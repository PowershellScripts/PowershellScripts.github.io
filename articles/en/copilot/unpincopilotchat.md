---
layout: page
title: 'How to unpin Copilot chat?'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-03-08'
---

This article shows you how to pin and unpin Copilot from the navigation bar, Teams and Windows taskbar.


# Default

Copilot Chat is pinned by default for users with a Microsoft 365 Copilot license. Copilot is pinned in the navigation bar of your Microsoft 365:

<img src="/articles/img/copilot37.png" width="800" alt="pinned copilot">



# How to unpin Copilot chat?

Navigate to the Settings in the Copilot Control System. You can find them in the Admin Center under the tab Copilot.


Direct link to your settings: [https://admin.microsoft.com/#/copilot/settings](https://admin.microsoft.com/#/copilot/settings) . The link works for every Microsoft 365 tenant.


Requires Global Administrator permission.

<img src="/articles/img/copilot35.png" width="400">



Any changes you make (pin, unpin) can take up to **48 hours** to go into effect. 



Additionally to those settings, you can pin Copilot in

* Teams
* Windows taskbar


# Teams

You can unpin Copilot using the same [app policies](https://learn.microsoft.com/en-us/microsoftteams/teams-app-setup-policies#pin-apps) you are using for every other app. Navigate to **Teams Admin Center** and go to **Manage Apps**

<img src="/articles/img/copilot44.png" width="800" alt="Teams admin center">


Find Copilot and make it unavailable to your users. 

<img src="/articles/img/copilot47.png" width="800" alt="Teams admin center">


Under **Setup Policies** define whether the Copilot should be pinned by default and for which users. Verify whether one of the policies is not pinning the Copilot in Teams.

<img src="/articles/img/copilot46.png" width="400" alt="Teams admin center">


### Windows taskbar

Follow this guidance on how to configure Copilot in the Windows taskbar for your users using Intune:
[https://learn.microsoft.com/en-us/windows/configuration/taskbar/pinned-apps?tabs=intune&pivots=windows-11](https://learn.microsoft.com/en-us/windows/configuration/taskbar/pinned-apps?tabs=intune&pivots=windows-11)





# See Also

[How to adjust Copilot settings?](https://powershellscripts.github.io/articles/en/copilot/adjustcopilotsettings/)

[Copilot Chat vsus. Microsoft 365 Copilot What's the difference?](https://techcommunity.microsoft.com/discussions/microsoft365copilot/copilot-chat-vsus-microsoft-365-copilot-whats-the-difference/4382855)

