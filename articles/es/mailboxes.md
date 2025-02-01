---
layout: page
title: 'Exchange Online: ¿A qué buzones tiene acceso el usuario?'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-02-01'
---

**El artículo describe una forma de verificar los derechos de acceso de un usuario a los buzones de otras personas.**

## Verificar quién tiene acceso a un solo buzón

Para verificar quién tiene acceso a un solo buzón, ejecuta el siguiente cmdlet:

```powershell
Get-MailboxPermission -Identity arleta
```

*arleta* - identidad del buzón, por ejemplo: arleta@testova365.onmicrosoft.com

En la captura de pantalla a continuación, puedes ver que user2@testova365.onmicrosoft.com tiene derechos de acceso completo (*FullAccess*) en el buzón. Pero, ¿cómo comprobar a qué buzones tiene acceso user2 y cuántos son?

<img src="/articles/img/mail.png" width="600" >  

<br/><br/>

## Verificar a qué buzones tiene acceso un usuario

No existe un cmdlet directo, pero podemos iterar a través de todos los buzones existentes y verificar los derechos de un usuario específico:  


```powershell
$mailboxes =  Get-Mailbox -Resultsize Unlimited

foreach($mailbox in $mailboxes){
    Get-MailboxPermission -Identity $mailbox.Identity -User user2@testova365.onmicrosoft.com
    }
```

<img src="/articles/img/mail2.png" width="600" > 

La columna *AccessRights* muestra el nivel de acceso a cada buzón individual. El buzón propio del usuario no se mostrará.

<br/><br/>

## Exportar a CSV
El informe sobre los permisos de un usuario puede exportarse a un archivo CSV:

```powershell
foreach($mailbox in $mailboxes){
    Get-MailboxPermission -Identity $mailbox.Identity -User user2@testova365.onmicrosoft.com | export-csv c:\maiperms.csv -Append
    }
```

<br/><br/>

## Otros Idiomas
Este artículo está disponible en otros idiomas:  

[Exchange Online: What mailboxes has User access to? (en-US)](https://powershellscripts.github.io/articles/en/Other/mailboxes/)

[Exchange Online: Do jakich skrzynek użytkownik ma dostęp? (pl-PL)](https://powershellscripts.github.io/articles/pl/mailboxes/)

[Exchange Online: lister les boîtes aux lettres auxquelles un utilisateur a accès (fr-FR)](https://powershellscripts.github.io/articles/fr/mailboxes/)




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