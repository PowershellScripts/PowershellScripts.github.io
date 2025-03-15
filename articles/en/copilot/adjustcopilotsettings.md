---
layout: page
title: 'How to adjust Copilot settings?'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-02-23'
---

## Copilot Settings 


The Settings in the Copilot Control System offers various configuration options. You can find them in the Admin Center under the tab Copilot.
Many settings redirect to other admin centers. Mind you, you might need extra permissions for access.

Direct link to your settings: [https://admin.microsoft.com/#/copilot/settings](https://admin.microsoft.com/#/copilot/settings) . The link works for every Microsoft 365 tenant.

- Copilot diagnostic logs  
- Copilot image generation  
- Copilot in Bing, Edge, and Windows  
- Copilot in Edge  
- Copilot in Power Platform and Dynamics 365  
- Copilot in Teams meetings  
- Copilot in Viva Engage  
- Copilot in Viva Goals  
- Copilot in Viva Insights  
- Copilot in Viva Pulse  
- Copilot pay-as-you-go billing  
- Data security and compliance  
- Extensions  
- Microsoft 365 Copilot self-service purchases  
- Pin Microsoft 365 Copilot Chat  
- Recommendations for Microsoft 365 Copilot licensing  
- Web search for Microsoft 365 Copilot and Microsoft 365 Copilot Chat  


The exact number of settings depends on your tenant, the available licenses, and your own permissions.

#### For example:

<img src="/articles/img/copilot11.png" width="800">


#### With Viva licensing:

<img src="/articles/img/copilot23.png" width="800">


#### As a SharePoint Admin:

<img src="/articles/img/copilot24.png" width="800">




<br/><br/>

# Copilot image generation

Copilot image generation decides whether Copilot can suggest generated images or only stock images.
There are 2 options to choose from:

* **Allow Copilot image generation (recommended)**. Users can create images in Microsoft 365 apps and Designer
* **Don't allow.** Copilot will only provide stock or brand images when prompted.


<img src="/articles/img/copilot31.png" width="800">


<br/><br/>


# Copilot in Power Platform and Dynamics 365

Manages settings specific to agents and Copilot agents in Power Platform and Dynamics 365 products. It redirects to Power Platform Admin Center where you can see more of the related settings:


<img src="/articles/img/copilot32.png" width="800">


In the Power Platform Admin Center Copilot in Power Apps you can


* Enable Copilot preview features for people who make apps

* Allow Copilot authors to publish from Copilot Studio when AI features are enabled. (Publish bots with AI features)


<img src="/articles/img/copilot41.png" width="400">


## Data regions

Copilots and generative AI features aren't available in all regions and languages. In some cases, even where there is capacity in the region, data must still move outside of the region for availability reasons. For this reason and depending on where your environment is hosted, you might need to allow data movement across regions to use Copilots and generative AI features.
When you allow data movement across regions, your inputs (prompts) and outputs (results) might move outside of your region to the location where the generative AI feature is hosted. Microsoft, however, promise that [they don't use your data to train, retrain, or improve Azure OpenAI Service foundation models.](https://learn.microsoft.com/en-us/power-platform/admin/geographical-availability-copilot?WT.mc_id=ppac_inproduct_settings&tabs=new#regions-where-data-is-processed-for-copilots-and-generative-ai-features).
Special rules apply for Europe. For EU Data Boundary Services, processing of Customer Data and Personal Data remains within the EU Data Boundary as committed in the Product Terms.

The following table shows data residency for storage and data processing at the time of writing this article (February 2025). For the latest information please refer to [Learn.Microsoft.com](https://learn.microsoft.com/en-us/power-platform/admin/geographical-availability-copilot?WT.mc_id=ppac_inproduct_settings&tabs=new#regions-where-data-is-processed-for-copilots-and-generative-ai-features)

| Region where your Power Platform or Dynamics 365 environment is hosted | Region where Azure OpenAI Service is hosted        | Region where data is stored and processed for Bing Search |
|-------------------------------------------------------------------------|----------------------------------------------------|----------------------------------------------------------|
| United States                                                          | In region*                                         | United States                                            |
| Europe**                                                               | Spain, Sweden, or Switzerland                     | United States                                            |
| France                                                                 | Spain, Sweden, or Switzerland                     | United States                                            |
| Germany                                                                | Spain, Sweden, or Switzerland                     | United States                                            |
| Norway                                                                 | Spain, Sweden, or Switzerland                     | United States                                            |
| Switzerland                                                            | Spain, Sweden, or Switzerland                     | United States                                            |
| Asia                                                                   | United States                                     | United States                                            |
| Brazil                                                                 | United States                                     | United States                                            |
| Canada                                                                 | United States                                     | United States                                            |
| Japan                                                                  | United States                                     | United States                                            |
| Korea                                                                  | United States                                     | United States                                            |
| Singapore                                                              | United States                                     | United States                                            |
| South Africa                                                           | United States                                     | United States                                            |
| United Arab Emirates                                                   | United States                                     | United States                                            |
| Australia                                                              | In region* or United States                      | United States                                            |
| India                                                                  | In region* or United States                      | United States                                            |
| United Kingdom                                                         | In region*, Spain, Sweden, or Switzerland         | United States                                            |
| Government cloud (GCC, GCC High)                                       | In region*                                         | United States                                            |

> *Within the geographical region of your Power Platform or Dynamics 365 environment
> **Note that your Power Platform and Dynamics 365 environments are hosted in the EU Data Boundary, we use an Azure OpenAI endpoint in the same boundary.

<sup> Source: https://learn.microsoft.com/en-us/power-platform/admin/geographical-availability-copilot?WT.mc_id=ppac_inproduct_settings&tabs=new </sup>



You can view the settings for your regions in Power Platform Admin >> Copilot >> Governance:


<img src="/articles/img/copilot42.png" width="400">


<br/><br/>


# Extensions

Under the Copilot Extensions tab you can manage who can use Copilot with installed apps and extensions from Microsoft or other providers.

<img src="/articles/img/copilot33.png" width="800">


<br/><br/>


# Microsoft 365 Copilot self-service trials and purchases

Controls product trials and purchases to enable for end users in your organization.

<img src="/articles/img/copilot34.png" width="800">


<br/><br/>



# Pin Microsoft 365 Copilot Chat

Sets whether the Copilot should be pinned on the navigation bar or optional for the users. Requires Global Administrator permission.

<img src="/articles/img/copilot35.png" width="400">



If you are just starting your journey with Copilot, remember that Copilot Chat is pinned by default for users with a Microsoft 365 Copilot license.

Any changes you make (pin, unpin) can take up to **48 hours** to go into effect. 

Where is and what is th navigation bar? Well, it's the menu on the left. That's how Copilot looks pinned on the navigation bar:

<img src="/articles/img/copilot37.png" width="800" alt="pinned copilot">



Additionally to those settings, you can pin Copilot in

* Teams
* Windows taskbar


<br/><br/>

### Teams

You can pin Copilot using the same [app policies](https://learn.microsoft.com/en-us/microsoftteams/teams-app-setup-policies#pin-apps) you are using for every other app. Navigate to **Teams Admin Center** and go to **Manage Apps**

<img src="/articles/img/copilot44.png" width="800" alt="Teams admin center">

<br/>

Find Copilot and make it available to your users. You can choose a selected group or everyone.

<img src="/articles/img/copilot47.png" width="800" alt="Teams admin center">


<br/>

Under **Setup Policies** define whether the Copilot should be pinned by default and for which users:

<img src="/articles/img/copilot46.png" width="400" alt="Teams admin center">


<br/><br/>

### Windows taskbar

Follow this guidance on how to configure Copilot in the Windows taskbar for your users using Intune:

[https://learn.microsoft.com/en-us/windows/configuration/taskbar/pinned-apps?tabs=intune&pivots=windows-11](https://learn.microsoft.com/en-us/windows/configuration/taskbar/pinned-apps?tabs=intune&pivots=windows-11)


<br/><br/>
---
<br/>

# Web search for Microsoft 365 Copilot and Microsoft 365 Copilot Chat


If enabled, Copilot can reference web content to improve the quality of its responses to user prompts.


### What does that mean?

1. **Expanded Knowledge Base:** Copilot accesses real-time web content to provide information beyond its existing training data.
2. **Dynamic Updates:** Ensures responses reflect recent changes, trends, or updates on a given topic.
3. **Contextual Awareness:** Enables the integration of additional details, examples, and references in its output.

<br/>

#### Examples

**Example 1**

**Prompt:** "What are the latest trends in artificial intelligence for 2025?"  
**Response:** Copilot uses web-based reports or articles discussing new AI breakthroughs, like advancements in generative AI or machine learning applications.

**Example 2**

**Prompt:** "What is the weather forecast for this weekend in New York City?"  
**Response:** Copilot fetches live weather updates.

**Example 3**

**Prompt:** "Which is better for my team, Jira or Trello?"  
**Response:** Copilot compares data from the vendors' sites. It can give you a table comparing features, pricing, reviews, and offer a concise summary for decision-making.

**Example 4**

**Prompt:** "What time is the next flight from London to New York City on British Airways?"  
**Response:** With web search setting on, Copilot retrieves live flight schedules and displays accurate timing from airports or airlines.


<br/>

### Benefits
- **Improved Accuracy:** Delivers responses based on the latest and most reliable web content.
- **Increased Efficiency:** Saves users time by performing complex searches.


<br/>

### Considerations
- **Reliability of Sources:** As with all AI searches, the quality of the response depends on the credibility of the web content Copilot accesses.

- **Privacy Concerns:** Administrators should ensure web referencing complies with data protection policies, particularly in secure environments.


Mind you the generated search query is different from the user's original prompt. It consists of a few words generated by the user's prompt. Some examples of the information that **isn't** included in the generated search query sent to the Bing search service:

* The user's entire prompt, unless the prompt is very short (for example, "local weather")

* Entire Microsoft 365 files (for example, emails or documents) or files uploaded into Copilot

* Entire web pages or PDFs summarized by Copilot in Microsoft Edge (only for Microsoft 365 Copilot Chat)

<sup> Based on: https://learn.microsoft.com/en-gb/copilot/microsoft-365/manage-public-web-access </sup>


<br/>

## Configuration

Use Cloud Policies to configure the web search setting.

Direct link to Cloud Policies:
[https://config.office.com/officeSettings/officePolicy/createv2](https://config.office.com/officeSettings/officePolicy/createv2)

The link works on every tenant.



<img src="/articles/img/copilot38.png" width="800" alt="pinned copilot">

<img src="/articles/img/copilot39.png" width="400" alt="pinned copilot">



<br/>



### The small print


<img src="/articles/img/copilot40.png" width="600" alt="pinned copilot">




<br/><br/>



# See Also


[Copilot Control System](https://powershellscripts.github.io/articles/en/copilot/controlsystem/)


[Copilot Chat vsus. Microsoft 365 Copilot What's the difference?](https://techcommunity.microsoft.com/discussions/microsoft365copilot/copilot-chat-vsus-microsoft-365-copilot-whats-the-difference/4382855)










<!-- Default Statcounter code for Copilot-all
https://powershellscripts.github.io/articles/en/copilot/controlsystem/
-->
<script type="text/javascript">
var sc_project=13093998; 
var sc_invisible=1; 
var sc_security="3e9d4e7b"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13093998/0/3e9d4e7b/1/"
alt="Web Analytics"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->