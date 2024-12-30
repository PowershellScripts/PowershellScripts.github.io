---
layout: page
title: 'Exchange Online: lister les boîtes aux lettres auxquelles un utilisateur a accès'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-12-30'
---


Après avoir créé une boîte aux lettres d'utilisateur, vous pouvez la modifier et définir des propriétés supplémentaires via le Centre d'administration Exchange (CAE) ou en utilisant l'environnement de ligne de commande Exchange Management Shell. Parmi ces modifications, il est possible de partager la boîte aux lettres avec un autre utilisateur. Une fois partagée, cet utilisateur pourra l'ajouter à sa liste de dossiers dans Outlook Web App ou l'ouvrir dans une nouvelle fenêtre de navigateur.  

<br/><br/>

## Prérequis

```powershell
Install-Module -Name ExchangeOnlineManagement
Connect-ExchangeOnline -UserPrincipalName votre_utilisateur@domaine.com
```


<br/><br/>

##  Vérifier qui a accès à une seule boîte aux lettres

Pour vérifier qui a accès à une boîte aux lettres spécifique, vous pouvez utiliser cette commande:


```powershell
Get-MailboxPermission -Identity arleta
```

Dans la capture d’écran, vous pouvez voir que l’utilisateur **user2@testova365.onmicrosoft.com** dispose des droits de **Contrôle total** (FullAccess) sur la boîte aux lettres. Mais comment peut-on vérifier à combien et à quelles boîtes aux lettres l’utilisateur **user2** a accès ?



<img src="/articles/img/mail.png" width="600" > 


<br/><br/>

## Vérifier à quelles boîtes aux lettres l'utilisateur a accès

Il n'y a pas une seule phrase, qui pouvait vérifier ça, mais on peut créer un loop. Ce loop va chercher dans chaque boîte aux lettres et vérifier, si notre utilisateur y a l'accès:


```powershell
$mailboxes =  Get-Mailbox -Resultsize Unlimited

foreach($mailbox in $mailboxes){
    Get-MailboxPermission -Identity $mailbox.Identity -User user2@testova365.onmicrosoft.com
    }
```

<img src="/articles/img/mail2.png" width="600" > 

À gauche, vous voyez les noms des boîtes aux lettres auxquelles l’utilisateur **user2** a accès. À droite, vous voyez les niveaux d’accès : **Contrôle total** ou **Lecture** (FullAccess, ReadPermission). La boîte aux lettres personnelle de l'utilisateur ne sera pas affichée.

<br/><br/>

## Exporter vers CSV
On peut créer un rapport sur des droits de l'utilisateur et l'exporter dans un fichier avec cette phrase:


```powershell
foreach($mailbox in $mailboxes){
    Get-MailboxPermission -Identity $mailbox.Identity -User user2@testova365.onmicrosoft.com | export-csv c:\maiperms.csv -Append
    }

```

<br/><br/>

## Autres Langues

Cet article est disponible dans d'autres langues :


[Exchange Online: What mailboxes has User access to? (en-US)](https://powershellscripts.github.io/articles/en/Other/mailboxes/)
[Exchange Online: Do jakich skrzynek użytkownik ma dostęp? (pl-PL)](https://powershellscripts.github.io/articles/pl/mailboxes/)






<!-- Default Statcounter code for Mailboxes
https://powershellscripts.github.io/articles/en/Other/mailboxes/
-->
<script type="text/javascript">
var sc_project=13073408; 
var sc_invisible=1; 
var sc_security="66de07d8"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/13073408/0/66de07d8/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->




