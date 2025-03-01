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

<br/><br/>


# Check

You can check settings for the entire company

<img src="/articles/images/m365groupsettings2.PNG" width="600" >

or for the specific group

<img src="/articles/images/m365groupsettings3.PNG" width="600" > 

There are several templates you can use and modify:

<img src="/articles/images/m365groupsettings.PNG" width="600" > 

<br/><br/>


# Possible settings
Please remember that the list of settings can be extended or changed at any moment due to Microsoft updates or subscription upgrades

- **AllowToAddGuests**: Specifies whether group owners can add guest users to the group.
- **EnableAccessCheckForPrivilegedApplicationUpdates**: Ensures privileged application updates undergo access checks for enhanced security.
- **BannedPasswordCheckOnPremisesMode**: Defines the mode for checking banned passwords in on-premises environments.
- **EnableBannedPasswordCheckOnPremises**: Activates banned password checks for on-premises systems.
- **EnableBannedPasswordCheck**: Enables the functionality to check for banned passwords to enhance security.
- **LockoutDurationInSeconds**: Specifies the duration in seconds for which an account is locked out after multiple failed login attempts.
- **LockoutThreshold**: Sets the number of failed login attempts that trigger an account lockout.
- **BannedPasswordList**: A list of passwords that are prohibited for use within the organization.
- **NewUnifiedGroupWritebackDefault**: Determines the default setting for writing back new unified groups to on-premises directories.
- **EnableMIPLabels**: Enables Microsoft Information Protection labels for classifying and protecting data.
- **CustomBlockedWordsList**: Specifies a list of words that are blocked from being used in group names or descriptions.
- **EnableMSStandardBlockedWords**: Enables the standard list of blocked words provided by Microsoft.
- **ClassificationDescriptions**: Provides descriptions for the classification labels used within the organization.
- **DefaultClassification**: Sets the default classification label for new groups.
- **PrefixSuffixNamingRequirement**: Defines required prefixes or suffixes for group names.
- **AllowGuestsToBeGroupOwner**: Specifies if guest users can be assigned as group owners.
- **AllowGuestsToAccessGroups**: Determines if guest users are allowed to access group content.
- **GuestUsageGuidelinesUrl**: URL to the guidelines for guest user usage.
- **GroupCreationAllowedGroupId**: Identifies the group of users who are allowed to create new groups.
- **UsageGuidelinesUrl**: URL to the usage guidelines for groups.
- **ClassificationList**: Defines the list of available classification labels for groups.
- **EnableGroupCreation**: Specifies if group creation is enabled for users.
- **CustomBlockedSubStringsList**: A list of substrings that are blocked from being used in group names or descriptions.
- **CustomBlockedWholeWordsList**: A list of whole words that are blocked from being used in group names or descriptions.
- **CustomConditionalAccessPolicyUrl**: URL to the custom conditional access policy.
- **CustomAllowedSubStringsList**: A list of substrings that are allowed in group names or descriptions, overriding blocked words.
- **CustomAllowedWholeWordsList**: A list of whole words that are allowed in group names or descriptions, overriding blocked words.
- **DoNotValidateAgainstTrademark**: Specifies whether group names should be validated against trademarked terms.
- **EnableGroupSpecificConsent**: Enables specific consent settings for individual groups.
- **BlockUserConsentForRiskyApps**: Prevents users from consenting to risky applications.
- **EnableAdminConsentRequests**: Allows users to request admin consent for applications.
- **ConstrainGroupSpecificConsentToMembersOfGroupId**: Limits group-specific consent to members of a specified group.


<br/><br/>


# Set
Use Set-PnPMicrosoft365GroupSettings to set different settings. The full documentation can be found here: [Set-PnPMicrosoft365GroupSettings](https://pnp.github.io/powershell/cmdlets/Set-PnPMicrosoft365GroupSettings.html) 

When modifying Microsoft 365 Group Settings, make sure you use the correct template ID, and that the specific settings is available in that template. Not all settings are avaialble in all templates. You can check your templates using Get-PnPMicrosoft365GroupSettingTemplates.

Same as you could check the settings for the entire Microsoft 365 tenant or a Microsoft 365 group, you can also set the settings for the entire tenant or a specific Microsoft 365 group.

```
Set-PnPMicrosoft365GroupSettings -Identity $groupSettingId -Values @{"EnableGroupCreation"="true"}

Set-PnPMicrosoft365GroupSettings -Identity $groupSettingId -Values @{"EnableGroupCreation"="true"} -Group $groupId
```
<br/><br/>


# Example

Using the cmdlet:

```
Set-PnPMicrosoft365GroupSettings -Identity $groupSettingId -Values @{"AllowToAddGuests"="true"}
```

we can modify the following setting:

<img src="/articles/images/m365groupsettings7.PNG" width="600" > 


<br/><br/>


# Possible errors
Some settings are for groups, some for the entire company. Don't mix them up or you will get errors

```
New-PnPMicrosoft365GroupSettings: Bad Request (400): ObjectSettingsTemplate '08d542b9-071f-4e16-94b0-74abb372e3d9' is not supported for DirectoryObjectClass 'Company'. paramName: SettingTemplateId, paramValue: 08d542b9-071f-4e16-94b0-74abb372e3d9, objectType: System.Guid

New-PnPMicrosoft365GroupSettings: Bad Request (400): ObjectSettingsTemplate '62375ab9-6b52-47ed-826b-58e47e0e304b' is not supported for DirectoryObjectClass 'Group'. paramName: SettingTemplateId, paramValue: 62375ab9-6b52-47ed-826b-58e47e0e304b, objectType: System.Guid
```

<img src="/articles/images/m365groupsettings5.PNG" > 

<br/><br/>


# See Also

[Get-PnPMicrosoft365GroupSettings](https://pnp.github.io/powershell/cmdlets/Get-PnPMicrosoft365GroupSettings.html)

[Get-PnPMicrosoft365GroupSettingTemplates](https://pnp.github.io/powershell/cmdlets/Get-PnPMicrosoft365GroupSettingTemplates.html)

[Set-PnPMicrosoft365GroupSettings](https://pnp.github.io/powershell/cmdlets/Set-PnPMicrosoft365GroupSettings.html)

[New-PnPMicrosoft365GroupSettings](https://pnp.github.io/powershell/cmdlets/New-PnPMicrosoft365GroupSettings.html)



<!-- Default Statcounter code for m365groupSettings
https://powershellscripts.github.io/articles/en/Other/m365groupsettings/
-->
<script type="text/javascript">
var sc_project=13025470; 
var sc_invisible=0; 
var sc_security="229b5a6c"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13025470/0/229b5a6c/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->