---
layout: page
title: 'Exchange Online - Do jakich skrzynek użytkownik ma dostęp?'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-12-30'
---

W artykule opisano sposób jak sprawdzić prawa dostępu użytkownika do skrzynek pocztowych innych osób.

##  Sprawdź, kto ma dostęp do pojedynczej skrzynki pocztowej

Ponisze polecenie sprawdza, kto ma dostęp do pojedynczej skrzynki pocztowej:


```powershell
Get-MailboxPermission -Identity arleta
```

arleta - tożsamość skrzynki pocztowej, np. arleta@testova365.onmicrosoft.com

Na zrzucie ekranu poniżej widać, że user2@testova365.onmicrosoft.com ma pełen dostep (FullAccess rights) do skrzynki pocztowej. Ale jak sprawdzić, do ilu i jakich skrzynek pocztowych User2 ma dostęp?

<img src="/articles/img/mail.png" width="600" > 


<br/><br/>

## Sprawdź, do jakich skrzynek pocztowych użytkownik ma dostęp

Nie ma bezpośredniego polecenia Powershell, ktre pozwoliłoby nam sprawdzic, do jakich skrzynek pocztowych użytkownik ma dostęp. Jest jednak mozliwość stworzenia pętli, ktora przeanalizuje prawa dla wszystkich istniejących skrzynek pocztowych i zwróci prawa określonego użytkownika:


```powershell
$mailboxes =  Get-Mailbox -Resultsize Unlimited

foreach($mailbox in $mailboxes){
    Get-MailboxPermission -Identity $mailbox.Identity -User user2@testova365.onmicrosoft.com
    }
```

<img src="/articles/img/mail2.png" width="600" > 

Kolumna AccessRights wyświetla poziom dostępu do każdej indywidualnej skrzynki pocztowej. Skrzynka pocztowa należąca do danego użytkownika nie pojawi się w wynikach wyszukiwania.

<br/><br/>

## Eksport do CSV
Raport nt. uprawnień użytkownika może być wyeksportowany do pliku csv:


```powershell
foreach($mailbox in $mailboxes){
    Get-MailboxPermission -Identity $mailbox.Identity -User user2@testova365.onmicrosoft.com | export-csv c:\maiperms.csv -Append
    }

```

<br/><br/>

## Powiazane artykuly
Artykul jest dostepny w innych jezykach:

Exchange Online: What mailboxes has User access to? (en-US)

Exchange Online: lister les boîtes aux lettres auxquelles un utilisateur a accès (fr-FR)