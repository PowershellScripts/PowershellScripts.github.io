---
layout: page
title: 'Microsoft 365: compare admin role permissions'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-11-09'
---


# Compare Roles

In Microsoft 365 admin center you can now compare various admin roles and select the one with minimum privilege for your admin accounts. The comparison lists detailed permissions of each role, and can be used for Compliance or ISO Documentation.





<img src="/articles/img/compareroles.PNG" >

<br/><br/><br/>

Navigate to Microsoft 365 admin center and select **Role Assignments** tab. You will see a list of admin roles. Select up to 3 admin roles and choose **Compare roles**

<br/>

<img src="/articles/img/compareroles2.PNG" >

<br/><br/><br/>

The output provides a detailed comparison of the selected roles and their associated permissions. It highlights the specific actions and access levels granted to each role. This allows for a clear understanding of the differences between roles and their respective permissions.

<br/>


<img src="/articles/img/compareroles6.PNG" >

<img src="/articles/img/compareroles3.PNG" >





<img src="/articles/img/compareroles4.PNG" >
<img src="/articles/img/compareroles5.PNG" >




# Sample Comparison

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






