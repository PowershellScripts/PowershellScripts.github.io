<h2>Introduction</h2>

As of November 2022, Cross Tenant OneDrive Migration is available as an add-on feature to Microsoft 365 subscription. User licenses are per migration (one-time fee) and can be assigned either on the source or target user object. This license also covers Cross-tenant mailbox migration. That means that now you can migrate users' content across tenants with a single Powershell cmdlet, without complex custom scripts or third-party solutions.


<h2>Step 1: Meet the licensing requirements</h2>
Acquire the add-on licenses.

<h2>Step 2: Disable OneDrive creation on Target Tenant</h2>

**Before** the users log in to your target tenant and automatically create their OneDrives, make sure you disable the possibility to create OneDrive for them. You do not have to disable the option for the entire tenant. Just make sure that the group of users you are about to migrate, have not accidentally created their sites by logging in and clicking on the OneDrive icon. Navigate to https://TENANT-admin.sharepoint.com/_layouts/15/TenantProfileAdmin/ManageUserProfileServiceApplication.aspx . Under **Manage User Permissions**, remove *Everyone except external users*. Instead, add a specific group to allow only a subset of licensed users to create a OneDrive.
If a OneDrive site already exists for the user in the target tenant, the migration will not succeed. It is not possible to overwrite an existing site.

<h2>Step 3: Precreate user accounts on Target Tenant</h2>
In the target tenant you are migrating to, precreate user accounts.

<h2>Step 4: Review OneDrive sizes on Source Tenant</h2>
Each OneDrive account can have a maximum of 2 TB of content or 1 million items. The 1 million items includes also file **versions**. That means that if you have 10'000 files with 500 versions each, the migration transfer will fail. 

<h2>Step 5: Review legal holds on Source Tenant</h2>
Any OneDrive accounts with a Hold policy applied will be blocked from migration.

<h2>Step 6: Connect to SPO Tenat</h2>
Using SharePoint Online Management Shell connect to both source and target tenant. Make sure you open two Powershell sessions (two Powershell windows). Connect-SPOService cmdlet disconnects any previous existing session. If you attempt to run these cmdlets in a single window, the process will fail!

<h2>Step 7: Run the trust commands</h2>
On the source tenant, execute this command to initiate a trust request to the target tenant:
Set-SPOCrossTenantRelationship -Scenario MnA -PartnerRole Target -PartnerCrossTenantHostUrl <TARGETCrossTenantHostUrl>

On the target tenant, execute this command to initiate a trust request to the target tenant:
Set-SPOCrossTenantRelationship -Scenario MnA -PartnerRole Source -PartnerCrossTenantHostUrl <SOURCECrossTenantHostUrl>

