---
layout: page
title: 'Audit sensitivity labels with Powershell'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2023-10-14'
---


## Vertraulichkeitsbezeichnungen
Vertraulichkeitsbezeichnungen (Sensitivity Labels) sind Teil der Microsoft Information Protection-Lösung. Vertraulichkeitsbezeichnungen klassifizieren und schützen die Daten Ihrer Organisation, indem sie geeignete Berechtigungen und Einschränkungen auf den klassifizierten Inhalt anwenden. Im Gegensatz zu Aufbewahrungslabeln, die an Orte wie alle Exchange-Postfächer veröffentlicht werden, werden Sensitivitätslabel an Benutzer oder Gruppen veröffentlicht. Das bedeutet, dass überall dort, wo die Labels unterstützt werden, Ihre Benutzer sie verwenden können. Apps, die Sensitivitätslabel unterstützen, zeigen sie den Benutzern und Gruppen an, für die sie veröffentlicht wurden. Die Sensitivitätslabel werden als bereits angewandte Labels angezeigt, wenn sie automatisch angewandt werden; oder als Labels, die angewandt werden können, wenn der Compliance-Administrator entschieden hat, dass sie von Benutzern angewandt werden können.

## Voraussetzungen
Installieren Sie das Exchange Online-Modul und verbinden Sie sich mit dem Security & Compliance PowerShell.

```powershell
Install-Module -Name ExchangeOnlineManagement -RequiredVersion 2.0.5
Connect-IPPSSession -UserPrincipalName User@contoso.com
```

## Überprüfen Sie vorhandene Labels
Verwenden Sie das Get-Label Cmdlet, um die verfügbaren Labels in Ihrer Umgebung, deren Geltungsbereiche und Prioritäten anzuzeigen.

```powershell
Get-Label
```
<img src="/articles/images/sens30.PNG" width="400">


## Alle Details zu einem Label abrufen

```powershell
Get-Label -Identity keyword-label | fl
```

<img src="/articles/images/sens31.PNG" width="400">

## Alle Labelaktionen anzeigen
Wenn Sie den Parameter -IncludeDetailedLabelActions verwenden, sehen Sie die speziellen Aktionen, die jedem Label zugewiesen sind.
<br/>
<img src="/articles/images/sens32.PNG" width="400">

## Welches Label verursacht Probleme
Wenn Sie unsicher sind, welches Label eine bestimmte Richtlinie anwendet, können Sie überprüfen, welche Labels welche Aktionen ausführen.

```powershell

Get-Label -IncludeDetailedLabelActions $true | select Applywatermarkingtext, displayname
```

<img src="/articles/images/sens34.PNG" width="400>

```powershell

Get-Label -IncludeDetailedLabelActions $true | select EncryptionEnabled, displayname
```

<img src="/articles/images/sens35.PNG" width="400>

## Automatische Labelzuweisung finden
Jedes Label verfügt über eine Eigenschaft namens "Capabilities" (Fähigkeiten). Mithilfe dieser Eigenschaft können Sie herausfinden, welche Labels automatisch auf den Inhalt angewendet werden.

```powershell

Get-Label | select Capabilities, DisplayName
```

## Überprüfungsmatrix für Labelaktionen
Die Überprüfungsmatrix ermöglicht es Ihnen, einen vollständigen Überblick über Labels, deren Aktionen, Fähigkeiten und Einstellungen zu einem bestimmten Zeitpunkt zu erhalten. Es handelt sich um eine einfache Excel-Datei, die jedoch nützlich sein kann, wenn Ihre Organisation die Einstellungen aus Compliance-Gründen speichern muss.

<img src="/articles/images/sens36.PNG" width="400">
<img src="/articles/images/sens37.PNG" width="400">

## Siehe auch
[M365 Information protection: Understanding Sensitivity labels vs sensitive information types](https://social.technet.microsoft.com/wiki/contents/articles/54457.m365-information-protection-understanding-sensitivity-labels-vs-sensitive-information-types.aspx)

[Sensitivity labels: Enable labels for groups and sites](https://social.technet.microsoft.com/wiki/contents/articles/54499.sensitivity-labels-enable-labels-for-groups-and-sites.aspx)






<!-- Default Statcounter code for Audit sensitivity labels
with PS
https://powershellscripts.github.io/articles/en/InformationProtection/Audit%20your%20sensitivit
-->
<script type="text/javascript">
var sc_project=12764960; 
var sc_invisible=1; 
var sc_security="9671f253"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/12764960/0/9671f253/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->



