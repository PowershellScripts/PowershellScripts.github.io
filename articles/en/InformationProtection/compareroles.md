---
layout: page
title: 'Microsoft 365: compare admin role permissions'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-11-09'
---


# Compare Admin Roles

In Microsoft 365 admin center you can now compare various administrators' roles and select the one with minimum privilege for your admin accounts. The comparison lists detailed permissions of each role, and can be used for Compliance or ISO Documentation.





<img src="/articles/img/compareroles.PNG" alt="screenshot comparing admin roles">

<br/><br/><br/>

Navigate to Microsoft 365 admin center and select **Role Assignments** tab. You will see a list of admin roles. Select up to 3 admin roles and choose **Compare roles**

<br/>

<img src="/articles/img/compareroles2.PNG" alt="screenshot comparing admin roles">

<br/><br/><br/>

The output provides a detailed comparison of the selected roles and their associated permissions. It highlights the specific actions and access levels granted to each role. This allows for a clear understanding of the differences between roles and their respective permissions.

<br/>




<img src="/articles/img/compareroles3.PNG" alt="screenshot comparing admin roles">


<br/><br/><br/>

The permission comparison between selected admin roles can be exported by using the **Export comparison** button.


<br/>

<img src="/articles/img/compareroles7.PNG" alt="screenshot comparing admin roles">

<br/><br/><br/>

This will create a CSV file with a table and listed permissions.


<br/>

<img src="/articles/img/compareroles4.PNG" alt="screenshot comparing admin roles" >


<br/><br/><br/>


# Sample Comparison

<br/>

### Security Administrator vs Security Operator vs Security Reader

Sample comparison between Security Administrator, Security Operator and Security Reader. It lists only the first 35 granular permissions out of the 95 being compared at the time of writing this. 



| Permissions                                                                                                                     | Security Administrator | Security Operator | Security Reader |
|---------------------------------------------------------------------------------------------------------------------------------|------------------------|-------------------|-----------------|
| Read all properties on sign-in reports, including privileged properties                                                         | ✔                      | ✔                 | ✔               |
| Read all properties of provisioning logs                                                                                        | ✔                      | ✔                 | ✔               |
| Read all resources in Privileged Identity Management                                                                            | ✔                      | ✔                 | ✔               |
| Read standard properties of authorization policies                                                                              | ✔                      | ✔                 | ✔               |
| Read all properties on audit logs, including privileged properties                                                              | ✔                      | ✔                 | ✔               |
| Read basic properties on all resources in the Microsoft 365 admin center                                                        | ✔                      |                   | ✔               |
| Create and manage service requests in the Microsoft 365 admin center                                                            | ✔                      | ✔                 |                 |
| Read and configure Service Health in the Microsoft 365 admin center                                                             | ✔                      |                   | ✔               |
| Read Attack simulator reports in the Microsoft 365 Security center                                                              | ✔                      |                   |                 |
| Read standard properties of all resources in the Security & Compliance center                                                   | ✔                      |                   |                 |
| Read basic properties of custom rules that define network locations                                                             | ✔                      |                   |                 |
| microsoft.directory/multiTenantOrganization/tenants/standard/read                                                               | ✔                      |                   |                 |
| microsoft.directory/multiTenantOrganization/tenants/organizationDetails/read                                                    | ✔                      |                   |                 |
| microsoft.directory/multiTenantOrganization/standard/read                                                                       | ✔                      |                   |                 |
| microsoft.directory/multiTenantOrganization/joinRequest/standard/read                                                           | ✔                      |                   |                 |
| Read all resources in Microsoft Entra ID Protection                                                                             | ✔                      |                   |                 |
| Read all properties in entitlement management in Microsoft Entra ID                                                             | ✔                      |                   |                 |
| Read standard properties of federation configuration for domains                                                                | ✔                      |                   |                 |
| Read all properties of the backed up local administrator account credentials for Microsoft Entra ID joined devices, except the password | ✔                      |                   |                 |
| microsoft.directory/crossTenantAccessPolicy/partners/templates/multiTenantOrganizationPartnerConfiguration/standard/read        | ✔                      |                   |                 |
| microsoft.directory/crossTenantAccessPolicy/partners/templates/multiTenantOrganizationIdentitySynchronization/standard/read     | ✔                      |                   |                 |
| Read conditional access for policies                                                                                            | ✔                      |                   |                 |
| Read the "applied to" property for conditional access policies                                                                  | ✔                      |                   |                 |
| Read the owners of conditional access policies                                                                                  | ✔                      |                   |                 |
| Read BitLocker keys                                                                                                             | ✔                      |                   |                 |
| Create and manage support tickets in the Microsoft Entra admin center                                                           | ✔                      | ✔                 |                 |
| Read and configure service health in the Microsoft Entra admin center                                                           | ✔                      |                   |                 |
| Read all properties of attack simulation templates in Attack Simulator                                                          |                        |                   | ✔               |
| Read all properties of attack payloads in Attack Simulator                                                                      |                        |                   | ✔               |
| microsoft.networkAccess/allEntities/allProperties/read                                                                          |                        |                   | ✔               |
| Read basic properties on policies                                                                                               |                        |                   | ✔               |
| Read the "applied to" property for policies                                                                                     |                        |                   | ✔               |
| Read owners of policies                                                                                                         |                        |                   | ✔               |
| Read all properties of access reviews of all reviewable resources in Microsoft Entra ID                                         |                        |                   | ✔               |
| Manage all aspects of Microsoft Defender Advanced Threat Protection                                                             |                        | ✔                 |                 |




<br/><br/><br/>

### Compliance Administrator vs Compliance Data Administrator vs Global Reader

Sample comparison between Compliance Administrator, Compliance Data Administrator, and Global Reader. It lists only some of the first granular permissions out of all the permissions that are being compared. 

<img src="/articles/img/compareroles8.PNG" alt="screenshot comparing admin roles" >




<!-- Default Statcounter code for compare roles
https://powershellscripts.github.io/articles/en/InformationProtection/compareroles/
-->
<script type="text/javascript">
var sc_project=13062630; 
var sc_invisible=0; 
var sc_security="aedba8fe"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13062630/0/aedba8fe/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->