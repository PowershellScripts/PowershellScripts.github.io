---
layout: page
title: 'Compare SharePoint tenant settings'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-11-17'
---



# TL; DR;

You can find the script on [Github here](https://github.com/PowershellScripts/SharePointOnline-ScriptSamples/tree/develop/Tenant%20Settings/Compare2tenants) .


# Why

From comparing configurations between INT and PROD environments, to migrating SharePoint settings, to troubleshooting issues, there are numerous reasons to compare the settings of two Microsoft 365 tenants.

1. Migration Validation
During tenant-to-tenant migrations, comparing SharePoint settings between two tenants ensures that critical configurations from source are replicated correctly in the target tenant.

2. Compliance and Security Audits
Comparing Microsoft 365 tenants helps identify discrepancies in security and compliance settings to ensure both environments meet organizational or regulatory requirements.

3. Troubleshooting and Issue Resolution
Differences in tenant settings may explain functionality issues or inconsistencies in user experiences between tenants. If it works in your dev tenant, why doesn't it want to work in the Customer's?

4. Standardization Across Organizations
In organizations with multiple Microsoft 365 tenants (e.g., after mergers or acquisitions), comparing settings helps align configurations for consistency and streamlined operations.

5. Environment Synchronization
Ensures Microsoft 365 tenants used for testing or development (like a sandbox, TEST, UAT or INT), are configured similarly to production for accurate testing and validation.

6. Identifying Misconfigurations
Quickly highlights SharePoint settings that may have been inadvertently changed or left default when they should be customized.

7. Understanding Impact of Policies
If a new policy or feature is enabled in one tenant, comparing with a baseline tenant helps evaluate its impact or appropriateness.




# How - SPO Management Shell


# How - PnP Powershell

# How - CSOM






# See Also

[Get-SPOTenant](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/get-spotenant?view=sharepoint-ps)

[Get-PnPTenant](https://pnp.github.io/powershell/cmdlets/Get-PnPTenant.html)
