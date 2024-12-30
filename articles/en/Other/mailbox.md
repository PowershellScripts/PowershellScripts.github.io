---
layout: page
title: 'Exchange Online: What mailboxes User has access to?'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-12-30'
---

The article describes a way how to verify user's access rights to other people's mailboxes.

##  Check who has access to a single mailbox

In order to check who has access to a single mailbox, run the following cmdlet:


```powershell
Get-MailboxPermission -Identity arleta
```

arleta - identity of the mailbox, e.g. arleta@testova365.onmicrosoft.com

In the screenshot below you can see that user2@testova365.onmicrosoft.com has FullAccess rights on the mailbox. But how to check which and how many mailboxes user2 has access to?

<img src="/articles/img/mail.png" width="600" > 


<br/><br/>

## Check what mailboxes a user has access to

There is no direct cmdlet, but we can loop through all the existing mailboxes and verify specific user's rights:


```powershell
$mailboxes =  Get-Mailbox -Resultsize Unlimited

foreach($mailbox in $mailboxes){
    Get-MailboxPermission -Identity $mailbox.Identity -User user2@testova365.onmicrosoft.com
    }
```

<img src="/articles/img/mail2.png" width="600" > 

The AccessRights columns display the access level to each individual mailbox.  User's own mailbox will not be displayed.

<br/><br/>

## Export to CSV
The report on user's permissions can be exported to a CSV file:


```powershell
foreach($mailbox in $mailboxes){
    Get-MailboxPermission -Identity $mailbox.Identity -User user2@testova365.onmicrosoft.com | export-csv c:\maiperms.csv -Append
    }

```

<br/><br/>

## Other Languages
This article is available in other languages:

Exchange Online: Do jakich skrzynek użytkownik ma dostęp? (pl-PL)
Exchange Online: lister les boîtes aux lettres auxquelles un utilisateur a accès (fr-FR)