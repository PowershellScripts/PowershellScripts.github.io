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

**1. Migration Validation**

During tenant-to-tenant migrations, comparing SharePoint settings between two tenants ensures that critical configurations from source are replicated correctly in the target tenant.

**2. Compliance and Security Audits**

Comparing Microsoft 365 tenants helps identify discrepancies in security and compliance settings to ensure both environments meet organizational or regulatory requirements.

**3. Troubleshooting and Issue Resolution**

Differences in tenant settings may explain functionality issues or inconsistencies in user experiences between tenants. If it works in your dev tenant, why doesn't it want to work in the Customer's?


**4. Standardization Across Organizations**

In organizations with multiple Microsoft 365 tenants (e.g., after mergers or acquisitions), comparing settings helps align configurations for consistency and streamlined operations.

**5. Environment Synchronization**

Ensures Microsoft 365 tenants used for testing or development (like a sandbox, TEST, UAT or INT), are configured similarly to production for accurate testing and validation.

**6. Identifying Misconfigurations**

Quickly highlights SharePoint settings that may have been inadvertently changed or left default when they should be customized.

**7. Understanding Impact of Policies**

If a new policy or feature is enabled in one tenant, comparing with a baseline tenant helps evaluate its impact or appropriateness.


<br/><br/>

# Compare SharePoint tenant settings using SPO Management Shell

You need to connect to both tenants using the `Connect-SPOService` cmdlet and get the properties using the `Get-SPOTenant` cmdlet. Then compare all the properties. Two files will be genrated:
one - full list of all the compared SharePoint properties, called FullTenantComparison.csv
two - only the differences between the Microsoft 365 tenants, called TenantDifferences

```powershell
# Define a function to connect to a tenant and get properties
function Get-TenantProperties {
    param (
        [string]$AdminUrl
    )

    Write-Host "Connecting to $AdminUrl..." -ForegroundColor Cyan
    Connect-SPOService -Url $AdminUrl
    $tenantProperties = Get-SPOTenant | Select-Object *
    Disconnect-SPOService
    return $tenantProperties
}

# Define a function to compare two objects
function Compare-TenantProperties {
    param (
        [PSCustomObject]$Tenant1,
        [PSCustomObject]$Tenant2
    )

    $comparison = @()
    foreach ($property in $Tenant1.PSObject.Properties) {
        $propertyName = $property.Name
        $value1 = $Tenant1.$propertyName
        $value2 = $Tenant2.$propertyName

        $comparison += [PSCustomObject]@{
            PropertyName = $propertyName
            Tenant1Value = $value1
            Tenant2Value = $value2
            IsDifferent  = $value1 -ne $value2
        }
    }
    return $comparison
}

# Enter the admin URLs for the two tenants
$Tenant1AdminUrl = "https://tenant1-admin.sharepoint.com"
$Tenant2AdminUrl = "https://tenant2-admin.sharepoint.com"

# Get properties from both tenants
$Tenant1Properties = Get-TenantProperties -AdminUrl $Tenant1AdminUrl
$Tenant2Properties = Get-TenantProperties -AdminUrl $Tenant2AdminUrl

# Compare the properties
$ComparisonResults = Compare-TenantProperties -Tenant1 $Tenant1Properties -Tenant2 $Tenant2Properties

# Filter only the differences
$Differences = $ComparisonResults | Where-Object { $_.IsDifferent -eq $true }

# Output to console
Write-Host "Comparison complete." -ForegroundColor Green

# Export all settings compared
$ComparisonResults | Export-Csv -Path "TenantComparison_Full.csv" -NoTypeInformation -Encoding UTF8
Write-Host "Full comparison exported to TenantComparison_Full.csv" -ForegroundColor Cyan

# Export only differences
$Differences | Export-Csv -Path "TenantComparison_Differences.csv" -NoTypeInformation -Encoding UTF8
Write-Host "Differences exported to TenantComparison_Differences.csv" -ForegroundColor Cyan

```


<br/><br/>


# Compare SharePoint tenant settings using PnP Powershell

You need to connect to both tenants using the `Connect-PnPOnline` cmdlet and get the properties using the `Get-PnPTenant` cmdlet. Then compare all the properties. Two files will be genrated:
one - full list of all the compared SharePoint properties, called FullTenantComparison.csv
two - only the differences between the Microsoft 365 tenants, called TenantDifferences

```powershell

# Connect to the first tenant
$Tenant1Url = "https://tenant1-admin.sharepoint.com"
$Tenant2Url = "https://tenant2-admin.sharepoint.com"

Write-Host "Connecting to Tenant 1: $Tenant1Url" -ForegroundColor Cyan
Connect-PnPOnline -Url $Tenant1Url -Interactive
$Tenant1Properties = Get-PnPTenant | Select-Object -Property *

# Connect to the second tenant
Write-Host "Connecting to Tenant 2: $Tenant2Url" -ForegroundColor Cyan
Connect-PnPOnline -Url $Tenant2Url -Interactive
$Tenant2Properties = Get-PnPTenant | Select-Object -Property *

# Compare the tenant properties
Write-Host "Comparing properties..." -ForegroundColor Yellow
$Comparison = @()
$AllProperties = $Tenant1Properties.PSObject.Properties.Name

foreach ($Property in $AllProperties) {
    $Value1 = $Tenant1Properties.$Property
    $Value2 = $Tenant2Properties.$Property
    
    if ($Value1 -ne $Value2) {
        $Comparison += [PSCustomObject]@{
            PropertyName = $Property
            Tenant1Value = $Value1
            Tenant2Value = $Value2
        }
    }
}

# Output the differences
if ($Comparison.Count -gt 0) {
    Write-Host "Differences found:" -ForegroundColor Green
    $Comparison | Format-Table -AutoSize
    
    # Export differences to CSV
    $Comparison | Export-Csv -Path "TenantDifferences.csv" -NoTypeInformation
    Write-Host "Differences exported to TenantDifferences.csv" -ForegroundColor Green
} else {
    Write-Host "No differences found between the two tenants." -ForegroundColor Green
}

# Output all properties for full comparison
$FullComparison = @()
foreach ($Property in $AllProperties) {
    $FullComparison += [PSCustomObject]@{
        PropertyName = $Property
        Tenant1Value = $Tenant1Properties.$Property
        Tenant2Value = $Tenant2Properties.$Property
    }
}

# Export full comparison to CSV
$FullComparison | Export-Csv -Path "FullTenantComparison.csv" -NoTypeInformation
Write-Host "Full comparison exported to FullTenantComparison.csv" -ForegroundColor Green


```


<br/><br/>


# Compare SharePoint tenant settings using CSOM


First connect to both tenants and export the SharePoint tenant properties by loading `Microsoft.Online.SharePoint.TenantAdministration.Tenant` object for each Microsoft 365 tenant. Then compare all the properties. Two files will be genrated:
one - full list of all the compared SharePoint properties, called FullTenantComparison.csv
two - only the differences between the Microsoft 365 tenants, called TenantDifferences


```powershell

# Load required assemblies
Add-Type -Path "C:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.dll"
Add-Type -Path "C:\Program Files\Common Files\microsoft shared\Web Server Extensions\16\ISAPI\Microsoft.SharePoint.Client.Runtime.dll"

# Function to get tenant properties
function Get-TenantProperties {
    param (
        [string]$AdminUrl,
        [string]$Username,
        [string]$Password
    )

    $securePassword = ConvertTo-SecureString -String $Password -AsPlainText -Force
    $credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($Username, $securePassword)

    $context = New-Object Microsoft.SharePoint.Client.ClientContext($AdminUrl)
    $context.Credentials = $credentials

    # Load tenant properties
    $tenant = New-Object Microsoft.Online.SharePoint.TenantAdministration.Tenant($context)
    $context.Load($tenant)
    $context.ExecuteQuery()

    # Extract and return tenant properties
    return $tenant | Get-Member -MemberType Property | ForEach-Object {
        [PSCustomObject]@{
            PropertyName = $_.Name
            PropertyValue = $tenant.($_.Name)
        }
    }
}

# Connect to the first tenant
$Tenant1AdminUrl = "https://tenant1-admin.sharepoint.com"
$Tenant1Username = "admin1@tenant1.onmicrosoft.com"
$Tenant1Password = "YourPassword1"

Write-Host "Retrieving properties from Tenant 1: $Tenant1AdminUrl" -ForegroundColor Cyan
$Tenant1Properties = Get-TenantProperties -AdminUrl $Tenant1AdminUrl -Username $Tenant1Username -Password $Tenant1Password

# Connect to the second tenant
$Tenant2AdminUrl = "https://tenant2-admin.sharepoint.com"
$Tenant2Username = "admin2@tenant2.onmicrosoft.com"
$Tenant2Password = "YourPassword2"

Write-Host "Retrieving properties from Tenant 2: $Tenant2AdminUrl" -ForegroundColor Cyan
$Tenant2Properties = Get-TenantProperties -AdminUrl $Tenant2AdminUrl -Username $Tenant2Username -Password $Tenant2Password

# Compare tenant properties
Write-Host "Comparing properties..." -ForegroundColor Yellow
$Comparison = @()

foreach ($Property in $Tenant1Properties) {
    $Tenant2Value = ($Tenant2Properties | Where-Object { $_.PropertyName -eq $Property.PropertyName }).PropertyValue

    if ($Property.PropertyValue -ne $Tenant2Value) {
        $Comparison += [PSCustomObject]@{
            PropertyName  = $Property.PropertyName
            Tenant1Value  = $Property.PropertyValue
            Tenant2Value  = $Tenant2Value
        }
    }
}

# Output the differences
if ($Comparison.Count -gt 0) {
    Write-Host "Differences found:" -ForegroundColor Green
    $Comparison | Format-Table -AutoSize

    # Export differences to CSV
    $Comparison | Export-Csv -Path "TenantDifferences.csv" -NoTypeInformation
    Write-Host "Differences exported to TenantDifferences.csv" -ForegroundColor Green
} else {
    Write-Host "No differences found between the two tenants." -ForegroundColor Green
}

# Output all properties for full comparison
$FullComparison = $Tenant1Properties | ForEach-Object {
    $Tenant2Value = ($Tenant2Properties | Where-Object { $_.PropertyName -eq $_.PropertyName }).PropertyValue

    [PSCustomObject]@{
        PropertyName  = $_.PropertyName
        Tenant1Value  = $_.PropertyValue
        Tenant2Value  = $Tenant2Value
    }
}

# Export full comparison to CSV
$FullComparison | Export-Csv -Path "FullTenantComparison.csv" -NoTypeInformation
Write-Host "Full comparison exported to FullTenantComparison.csv" -ForegroundColor Green

```

<br/><br/>


# See Also

[Get-SPOTenant](https://learn.microsoft.com/en-us/powershell/module/sharepoint-online/get-spotenant?view=sharepoint-ps)

[Get-PnPTenant](https://pnp.github.io/powershell/cmdlets/Get-PnPTenant.html)






<!-- Default Statcounter code for compare tenants
https://powershellscripts.github.io/articles/en/spo/comparetenants/
-->
<script type="text/javascript">
var sc_project=13063502; 
var sc_invisible=0; 
var sc_security="24b3c5af"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13063502/0/24b3c5af/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->