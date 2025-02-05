---
layout: page
title: 'Activer la gestion des types de contenu'
hero_image: '/img/IMG_20220521_140146.jpg'

show_sidebar: false
hero_height: is-small
date: '2025-01-05'
---

## Pourquoi activer la gestion des types de contenu ?

Activer la gestion des types de contenu est essentiel pour personnaliser la manière dont des types spécifiques de contenu sont gérés et suivis dans votre site SharePoint. Cela est particulièrement utile si vous devez :

* Permettre ou limiter l’utilisation de types de contenu personnalisés dans les listes et bibliothèques.  
* Standardiser des types de documents comme les demandes d’équipement, bons de commande ou contrats de vente dans toute votre organisation.  

Les types de contenu dans SharePoint aident à organiser et à gérer divers types de documents ou éléments dans une liste ou une bibliothèque. Ils permettent aux propriétaires de sites de définir des champs spécifiques, des flux de travail, des modèles et des formulaires pour une création de contenu cohérente et efficace.

Par défaut, SharePoint inclut plusieurs types de contenu prédéfinis. Cependant, pour répondre aux besoins uniques de votre organisation, vous pourriez avoir besoin de créer des types de contenu personnalisés pour des types de documents ou des flux de travail spécifiques.  


<br/><br/>


## Interface Utilisateur

Comment activer ou désactiver la gestion des types de contenu manuellement :

1. Ouvrez la liste ou la bibliothèque où vous souhaitez modifier le paramètre.

2. Sur le côté droit, cliquez sur **Paramètres de la liste**.

   <img src="/articles/img/enablect.png" ><br/>

3. Dans les **Paramètres**, choisissez **Paramètres avancés**.

   <img src="/articles/img/enablect2.png" ><br/>

4. Dans la section **Types de contenu**, vous pouvez choisir d’autoriser ou non la gestion des types de contenu. **Oui** activera la gestion des types de contenu supplémentaires pour votre liste. Cela correspond à `$true` dans le script ci-dessous. **Non** signifie que les types de contenu supplémentaires et la gestion des types de contenu doivent être désactivés, tandis que **Non** correspond à `$false` :

   <img src="/articles/img/enablect3.png" ><br/>



   <br/><br/>



## PnP PowerShell

Pour activer ou désactiver la gestion des types de contenu dans une liste SharePoint à l'aide de PowerShell, vous pouvez utiliser les cmdlets PnP PowerShell ou SharePoint Online Management Shell. Voici comment procéder :

#### Pour une liste

```powershell
Connect-PnPOnline -Url "https://votrelocataire.sharepoint.com/sites/votresite" -UseWebLogin
Set-PnPList -Identity "Documents" -ContentTypesEnabled $true
```

<br/>

#### Pour toutes les listes de la collection de sites

Le script ci-dessous active la gestion des types de contenu pour toutes les listes et bibliothèques dans une collection de sites SharePoint Online :

```powershell
# Connectez-vous au site SharePoint
Connect-PnPOnline -Url "https://votrelocataire.sharepoint.com/sites/votresite" -UseWebLogin

# Récupérez toutes les listes du site
$lists = Get-PnPList

# Parcourez chaque liste et activez les types de contenu
foreach ($list in $lists) {
    Set-PnPList -Identity $list -ContentTypesEnabled $true
    Write-Host "Types de contenu actives pour la liste : $($list.Title)"
}
```





<!-- Default Statcounter code for SPO enable ct
https://powershellscripts.github.io/articles/en/spo/enablect/
-->
<script type="text/javascript">
var sc_project=13073435; 
var sc_invisible=1; 
var sc_security="a5c7940a"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13073435/0/a5c7940a/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->