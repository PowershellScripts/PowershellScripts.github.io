---
layout: page
title: 'Microsoft 365 group settings'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-07-21'
---


# Intro

Microsoft 365 group settings configured using the Set-PnPMicrosoft365GroupSettings cmdlet can be viewed and managed in various places within the Microsoft 365 admin center. However, not all settings may have a direct user interface (UI) counterpart, as some configurations are more advanced and typically managed through PowerShell.

# Get

You can check settings for the entire company

<img src="/articles/images/m365groupsettings2.PNG" width="600" >

or for the specific group

<img src="/articles/images/m365groupsettings3.PNG" width="600" > 

# Possible settings
Please remember that the list of settings can be extended or changed at any moment due to Microsoft updates or subscription upgrades

AllowToAddGuests
EnableAccessCheckForPrivilegedApplicationUpdates
BannedPasswordCheckOnPremisesMode
EnableBannedPasswordCheckOnPremises
EnableBannedPasswordCheck
LockoutDurationInSeconds
LockoutThreshold
BannedPasswordList
NewUnifiedGroupWritebackDefault
EnableMIPLabels
CustomBlockedWordsList
EnableMSStandardBlockedWords
ClassificationDescriptions
DefaultClassification
PrefixSuffixNamingRequirement
AllowGuestsToBeGroupOwner
AllowGuestsToAccessGroups
GuestUsageGuidelinesUrl
GroupCreationAllowedGroupId
AllowToAddGuests
UsageGuidelinesUrl
ClassificationList
EnableGroupCreation
CustomBlockedSubStringsList
CustomBlockedWholeWordsList
CustomConditionalAccessPolicyUrl
CustomAllowedSubStringsList
CustomAllowedWholeWordsList
DoNotValidateAgainstTrademark
EnableGroupSpecificConsent
BlockUserConsentForRiskyApps
EnableAdminConsentRequests
ConstrainGroupSpecificConsentToMembersOfGroupId



# Set


# Example

<img src="/articles/images/SecureAzFunc/Github-SecureAzFunc1.PNG" width="600"  alt="Diagram showing OAuth communication where audience is the backend.Diagram showing OAuth communication where audience is the API Management gateway.">

# Possible errors
Some settings are for groups, some for the entire company. Don't mix them up or you will get errors

New-PnPMicrosoft365GroupSettings: Bad Request (400): ObjectSettingsTemplate '08d542b9-071f-4e16-94b0-74abb372e3d9' is not supported for DirectoryObjectClass 'Company'. paramName: SettingTemplateId, paramValue: 08d542b9-071f-4e16-94b0-74abb372e3d9, objectType: System.Guid

New-PnPMicrosoft365GroupSettings: Bad Request (400): ObjectSettingsTemplate '62375ab9-6b52-47ed-826b-58e47e0e304b' is not supported for DirectoryObjectClass 'Group'. paramName: SettingTemplateId, paramValue: 62375ab9-6b52-47ed-826b-58e47e0e304b, objectType: System.Guid


# See Also

[Get-PnPMicrosoft365GroupSettings](https://pnp.github.io/powershell/cmdlets/Get-PnPMicrosoft365GroupSettings.html)
[Get-PnPMicrosoft365GroupSettingTemplates](https://pnp.github.io/powershell/cmdlets/Get-PnPMicrosoft365GroupSettingTemplates.html)
[Set-PnPMicrosoft365GroupSettings](https://pnp.github.io/powershell/cmdlets/Set-PnPMicrosoft365GroupSettings.html)
[New-PnPMicrosoft365GroupSettings](https://pnp.github.io/powershell/cmdlets/New-PnPMicrosoft365GroupSettings.html)
