---
layout: page
title: 'Monitor Viva Engage conversations'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-02-16'
---




## Viva Engage Security 

To monitor Viva Engage conversations, you need to have administrative permissions or roles that allow you to manage the network settings, e.g. Viva Engage Admin or Network Admin. Here’s how to get started:  

1. Log in to Viva Engage  
- Navigate to [Microsoft Viva Engage](https://www.microsoft.com/en-us/microsoft-viva/engage) or access it via **Microsoft Teams** under the **Viva Engage tab**.  

2. Access the Admin Center  
- Click on the **gear icon** in the top-right corner (**Settings**).  
- Select **Edit network admin settings** from the dropdown menu.  

<img src="/articles/img/monitorviva3.png" width="700" alt="network admin settings">

3. Navigate to Monitoring Tools  
- In the Admin Center, look for the **Content and Security** section.  

<img src="/articles/img/monitorviva4.png" width="700" alt="screenshot showing network admin settings">


<br/><br/>

## Keyword Monitoring  

Keyword monitoring allows Viva Engage admin to track specific terms or phrases that might indicate issues, trends, or compliance violations. Here’s how to set it up:  

1. Enable Keyword Monitoring  
- In the Admin Center, go to **Content and Security > Keyword Monitoring**.  
- Click on **Add Keywords** to define the terms or phrases you want to monitor.  

<img src="/articles/img/monitorviva.png" width="400" alt="screenshot showing keywords monitoring">

2. Define Keywords Strategically  
Add keywords related to:  
- **Compliance Risks**: GDPR, PCI, HIPAA.  
- **Sensitive Topics**: Harassment, Confidential, Incident.  
- **Productivity Trends**: Feedback, Delay, Success.  

3. Review Alerts  
- Set up email notifications or alerts when monitored keywords are used in conversations.  
- Assign responsible moderators to review flagged posts promptly.  


>Email bodies will include the detected keyword and an inline view of the conversation for public communities. For private community conversations, a link to the detected conversation/message will be provided. Recipients must have access to the community to view the conversation.  



<br/><br/>

## Reporting Conversations  

Sometimes, specific conversations may violate community guidelines or pose risks to your organization. Viva Engage allows both users and administrators to report such conversations for review and resolution.  

#### Enabling Reporting  
Before reporting can be used, the feature must be enabled by administrators.  

<img src="/articles/img/monitorviva5.png" width="700" alt="screenshot showing reporting settings">

**For Administrators:**  
1. Log in to **Viva Engage** and go to the **Admin Center**.  
2. Navigate to **Content and Security > Report Conversations**.  
3. Set up notifications to ensure moderators are alerted when content is reported. 
4. Fill the **Pre-submission instructions for user** and **Post-submission instructions to user**

<br/>

For example:
> Before submitting a report, please follow these guidelines:  
> - **Review the Conversation:** Ensure the content genuinely violates guidelines or policies.  
> - **Understand Reporting Criteria:** Reports should align with organizational rules, such as flagging content for harassment, spam, or sensitive information exposure.  
> - **Consider Context:** Make sure the reported content doesn’t involve misunderstandings or taken-out-of-context remarks.  



<br/>

#### Reporting for Users  
Once reporting is enabled, users can report conversations or posts directly from the interface.  

**Steps to Report a Message or Post:**  
1. Hover over the message or post you want to report.  
2. Click the ellipsis menu (⋮) on the post.  
3. Select **Report This Conversation**  or **Report This Comment**  
4. Add additional comments if necessary to explain the issue.  


<img src="/articles/img/monitorviva6.png" width="500" alt="screenshot showing a viva engage post being reported">


<br/><br/>

## Monitor Activity with Community Analytics  

Community Analytics allows you to track the overall sentiment of conversations within your network. Positive, neutral, and negative sentiments are analyzed to help you understand the tone of discussions.  

**How to Access Sentiment Data:**  
1. Click on **See full community analytics**

<img src="/articles/img/monitorviva8.png" width="600" alt="screenshot showing community analytics">


You will be able to see Member activity, Views on posts, Messages posted, Reactions on messages as well as most common post types (Discussion, Poll, etc.) 

<img src="/articles/img/monitorviva9.png" width="600" alt="screenshot showing community analytics">


<br/><br/>


## Monitor Sentiment with Community Analytics 

Among other hidden gems in the Community Analytics you can monitor sentiment of yours users:

<img src="/articles/img/monitorviva7.png" width="600" alt="screenshot showing community analytics">




# See Also


[Viva Engage: View Analytics](https://powershellscripts.github.io/articles/en/Viva/viewanalytics/)
[View and manage analytics in Viva Engage](https://learn.microsoft.com/en-us/viva/engage/analytics)


<!-- Default Statcounter code for VEmonitorviva
https://powershellscripts.github.io/articles/en/Viva/monitorconversations/
-->
<script type="text/javascript">
var sc_project=13093070; 
var sc_invisible=1; 
var sc_security="cc81db00"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/13093070/0/cc81db00/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->