---
layout: page
title: 'Microsoft 365 Tips and Tricks'
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: ''
---
<script type="text/javascript" async src="//l.getsitecontrol.com/p43nv0p7.js"></script>

<style>


body {
	--h: 212deg;
	--l: 43%;
	--brandColor: hsl(var(--h), 71%, var(--l));
	font-family: Montserrat, sans-serif;
	margin: 0;
	background-color: whitesmoke;
}

a:link {
  color: black;
}

/* visited link */
a:visited {
  color: black;
}

/* mouse over link */
a:hover {
  color: black;
}

/* selected link */
a:active {
  color: black;
}

p {
	margin: 0;
	line-height: 1.6;
}

ol {
	list-style: none;
	counter-reset: list;
	padding: 0 1rem;
}

li {
	--stop: calc(100% / var(--length) * var(--i));
	--l: 62%;
	--l2: 88%;
	--h: calc((var(--i) - 1) * (180 / var(--length)));
	--c1: hsl(var(--h), 54%, var(--l));
	--c2: hsl(var(--h), 41%, var(--l2));
	
	position: relative;
	counter-increment: list;
	max-width: 85rem;
	margin: 2rem auto;
	padding: 1rem 1rem 1rem;
	box-shadow: 0.1rem 0.1rem 1.5rem rgba(0, 0, 0, 0.3);
	border-radius: 0.25rem;
	overflow: hidden;
	background-color: white;
}

li::before {
	content: '';
	display: block;
	width: 100%;
	height: 1rem;
	position: absolute;
	top: 0;
	left: 0;
	background: linear-gradient(to right, var(--c1) var(--stop), var(--c2) var(--stop));
}

h3 {
	display: flex;
	align-items: baseline;
	margin: 0 0 0rem;
	color: rgb(70 70 70);
}

h3::before {
	display: flex;
	justify-content: center;
	align-items: center;
	flex: 0 0 auto;
	margin-right: 1rem;
	width: 3rem;
	height: 3rem;
	content: "//";
	padding: 0rem;
	border-radius: 10%;
	background-color: var(--c1);
	color: white;
}

@media (min-width: 40em) {
	li {
		margin: 3rem auto;
		padding: 2rem 2rem 3rem;
	}
	
	h3 {
		font-size: 2.25rem;
		margin: 0 0 2rem;
	}

	sub {
		margin: 0 0 0rem;
		align-items: right;
	}
	
	h3::before {
		margin-right: 1.5rem;
	}
}


</style>

<ol style="--length: 5" role="list">
	<li style="--i: 7"><a href="https://powershellscripts.github.io/articles/en/Other/m365groupsettings/">
		<span style="display: block; text-align: right;">2024-07-21
		<h3>Modify Microsoft 365 group settings</h3></span>
		<p>Microsoft 365 group settings configured using the Set-PnPMicrosoft365GroupSettings cmdlet can be viewed and managed in various places within the Microsoft 365 admin center. However, not all settings may have a direct user interface (UI) counterpart, as some configurations are more advanced and typically managed through PowerShell.</p>
	</a></li>

		<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/Other/m365groupsettings/">
		<span style="display: block; text-align: right;">2024-08-01
		<h3>Add SharePoint site permissions to a group using PnP</h3></span>
		<p>Microsoft 365 group settings configured using the Set-PnPMicrosoft365GroupSettings cmdlet can be viewed and managed in various places within the Microsoft 365 admin center. However, not all settings may have a direct user interface (UI) counterpart, as some configurations are more advanced and typically managed through PowerShell.</p>
	</a></li>

	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/Other/m365groupsettings/">
		<span style="display: block; text-align: right;">2024-08-01
		<h3>SharePoint Claims Deep Dive</h3></span>
		<p>Microsoft 365 group settings configured using the Set-PnPMicrosoft365GroupSettings cmdlet can be viewed and managed in various places within the Microsoft 365 admin center. However, not all settings may have a direct user interface (UI) counterpart, as some configurations are more advanced and typically managed through PowerShell.</p>
	</a></li>

	<li style="--i: 7"><a href="https://powershellscripts.github.io/articles/en/Other/m365groupsettings/">
		<span style="display: block; text-align: right;">2024-07-21
		<h3>Modify Microsoft 365 group settings</h3></span>
		<p>Microsoft 365 group settings configured using the Set-PnPMicrosoft365GroupSettings cmdlet can be viewed and managed in various places within the Microsoft 365 admin center. However, not all settings may have a direct user interface (UI) counterpart, as some configurations are more advanced and typically managed through PowerShell.</p>
	</a></li>
 
	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/pnpsearchqcond/">
		<span style="display: block; text-align: right;">2024-07-14
		<h3>Conditional query in PNP Search Webpart</h3></span>
		<p>In SharePoint Search, especially when using the PnP (Patterns and Practices) Search Web Part, you can use the KQL (Keyword Query Language) search query template to build dynamic queries. The search query template allows for conditional logic using tokens and variables, but it's not as straightforward as using traditional programming "if" statements. By using the `{?{ }}` syntax in your PnP Search Web Part query template, you can introduce dynamic, conditional logic similar to "if" statements in programming.</p>
	</a></li>

	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/pnpsearchqex">
		<span style="display: block; text-align: right;">2024-07-14
		<h3>PnP Search query examples with KQL</h3></span>
		<p>KQL (Keyword Query Language) is a syntax used primarily within Microsoft products like SharePoint and Microsoft Search to formulate search queries. Understanding its basic rules and operators such as `AND`, `OR`, and other principles is crucial for constructing effective search queries. Here’s a breakdown of these rules with some examples.</p>
	</a></li>

	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/vivagraphapi/">
		<span style="display: block; text-align: right;">2024-06-02
		<h3>Manage Viva Engage with Graph API</h3></span>
		<p>Get, create and manage your Viva Engage communities using Graph API. Examples with Graph Explorer and explanation of the API. Read on... </p>
	</a></li>
	
	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/Closeconversation/">
		<span style="display: block; text-align: right;">2024-05-26
		<h3>Close conversations in Viva Engage</h3></span>
		<p>There is a variety of reasons why you should be closing Viva Engage conversations. A guide to keeping your Viva Engage community in orderly fashion</p>
	</a></li>

	<li style="--i: 7"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/HideTeamsPrompt">
		<span style="display: block; text-align: right;">2024-03-24
		<h3>Hide Teamify Prompt</h3></span>
		<p>In SharePoint, when a site is created without being associated with a Microsoft 365 group or a Microsoft Teams team, users typically encounter a teamify prompt displayed in the bottom left corner of the site interface. You can remove this teamify prompt using a PnP cmdlet... </p>
	</a></li>

 	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/CAMLQueryForListView/">
		<span style="display: block; text-align: right;">2024-03-24
		<h3>Easy way to create CAML Query for list view</h3></span>
		<p>When loading thousands of SharePoint list items, you may want to limit the number of retrieved results. The GetItems(CamlQuery) method allows you to define a Collaborative Application Markup Language (CAML) query that specifies which items to return. Creating a complex query, however, can present quite a challenge. To make sure every slash and value are exactly where they should be, it's easier to create a view using User Interface and then copy the CAML Query behind it. In this article, I am showing how to do it</p>
	</a></li>
 
	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/How%20to%20post%20as%20delegate/">
		<span style="display: block; text-align: right;">2024-02-03
		<h3>Viva: How to post as a delegate</h3></span>		
		<p>Now, you can post on behalf of another person in Viva Engage. This feature allows Viva Engage users to assign a delegate who can post on behalf of them. Configure it through Yammer’s settings section... </p>
	</a></li>
 
	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/Post%20as%20a%20leader%20to%20specific%20groups/">
		<span style="display: block; text-align: right;">2024-01-14
		<h3>Post as a leader to specific groups</h3></span>
		<p>The leadership feature in Viva allows you to identify the leaders in your organization and the leaders to reach their targeted audiences.</p>
	</a></li>
 
	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/Add%20Viva%20Engage%20to%20your%20SharePoint%20pages/">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>Add Viva Engage to your SharePoint pages</h3></span>
		<p>Viva Engage Conversations web part allows page viewers to participate in discussions without exiting the SharePoint environment. Use it to replace your old comments section with Viva Engage discussions.</p>
	</a></li>


 

 
	<li style="--i: 4"><a href="https://powershellscripts.github.io/articles/en/Viva/Post%20as%20a%20leader%20to%20specific%20groups/">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>[Add Viva Engage to your SharePoint pages](https://powershellscripts.github.io/articles/en/Viva/Closeconversation/)</h3></span>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Adipiscing diam donec adipiscing tristique risus.</p>
	</li>
		<li style="--i: 8">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>[Add Viva Engage to your SharePoint pages](https://powershellscripts.github.io/articles/en/Viva/Closeconversation/)</h3></span>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Adipiscing diam donec adipiscing tristique risus.</p>
	</li>







<br/><br/><br/><br/>
Links to my older Social Technet Articles:

<h3>Content Types</h3>

<li> <h3>[SharePoint Online content types in Powershell: Add](https://social.technet.microsoft.com/wiki/contents/articles/31051.sharepoint-online-content-types-in-powershell-add.aspx)</h3>
</li>

+ [SharePoint Online content types in Powershell: Get](http://social.technet.microsoft.com/wiki/contents/articles/31151.sharepoint-online-content-types-in-powershell-get.aspx)

* [SharePoint Online content types in Powershell: Edit](https://social.technet.microsoft.com/wiki/contents/articles/31444.sharepoint-online-content-types-in-powershell-edit.aspx)

* [SharePoint Online: Turn on support for multiple content types in a list or library using Powershell](https://social.technet.microsoft.com/wiki/contents/articles/30038.sharepoint-online-turn-on-support-for-multiple-content-types-in-a-list-or-library-using-powershell.aspx)

[SharePoint Online content types in PowerShell: Group property](http://social.technet.microsoft.com/wiki/contents/articles/31725.sharepoint-online-content-types-in-powershell-group-property.aspx)


[SharePoint Online: Remove site content type using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/30158.sharepoint-online-remove-site-content-type-using-powershell.aspx)

* [SharePoint Online content types in Powershell: Fields vs Field Links](https://social.technet.microsoft.com/wiki/contents/articles/31694.sharepoint-online-content-types-in-powershell-fields-vs-field-links.aspx)
* [SharePoint Online content types in Powershell: Known Errors](https://social.technet.microsoft.com/wiki/contents/articles/31770.sharepoint-online-content-types-in-powershell-known-errors.aspx)
* [Find content type ID in SharePoint Online using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/35717.find-content-type-id-in-sharepoint-online-using-powershell.aspx)
* [Content Type is still in use: Powershell to remove items and content types](https://social.technet.microsoft.com/wiki/contents/articles/35716.content-type-is-still-in-use-powershell-to-remove-items-and-content-types.aspx)

<br/><br/>
<h3>Versioning</h3>

* [Versioning and SharePoint: the Powershell perspective (part 1)](http://social.technet.microsoft.com/wiki/contents/articles/30115.versioning-and-sharepoint-the-powershell-perspective-part-1.aspx)

* [Versioning and SharePoint: the Powershell perspective (part 2)](https://social.technet.microsoft.com/wiki/contents/articles/30150.versioning-and-sharepoint-the-powershell-perspective-part-2.aspx)

* [Working with multiple items using Powershell](https://social.technet.microsoft.com/wiki/contents/articles/31382.sharepoint-online-working-with-multiple-items-using-powershell.aspx)
* [Delete unique permissions in multiple lists using CSOM](http://social.technet.microsoft.com/wiki/contents/articles/29556.sharepoint-online-delete-unique-permissions-in-multiple-lists-using-csom.aspx)
* [Create a report on SharePoint file versions](https://social.technet.microsoft.com/wiki/contents/articles/32613.create-a-report-on-sharepoint-file-versions.aspx)


<br/><br/>
<h3>Workflows</h3>

* [SharePoint Online: Verifying and modifying Flows Policy in site using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/39331.sharepoint-online-verifying-and-modifying-flows-policy-in-site-using-powershell.aspx)
* [SharePoint 2013/2016 Troubleshooting Workflow Farm](http://social.technet.microsoft.com/wiki/contents/articles/51716.sharepoint-20132016-troubleshooting-workflow-farm.aspx)
* [Reject? Approve? The third option in SharePoint Designer 2013 workflows](http://social.technet.microsoft.com/wiki/contents/articles/51865.reject-approve-the-third-option-in-sharepoint-designer-2013-workflows.aspx)
* [Error handling in Microsoft Flow](https://social.technet.microsoft.com/wiki/contents/articles/51961.error-handling-in-microsoft-flow.aspx)


<br/><br/>
<h3>Permissions & Sharing</h3>

* [SharePoint Online: How to change primary administrator for all site collections using PowerShell](http://social.technet.microsoft.com/wiki/contents/articles/30299.sharepoint-online-how-to-change-primary-administrator-for-all-site-collections-using-powershell.aspx)
* [Restoring and removing item permissions in subfolders for SharePoint Online using Powershell](http://social.technet.microsoft.com/wiki/contents/articles/36004.restoring-and-removing-item-permissions-in-subfolders-for-sharepoint-online-using-powershell.aspx)
* [SharePoint Online Sharing settings with CSOM](http://social.technet.microsoft.com/wiki/contents/articles/39365.sharepoint-online-sharing-settings-with-csom.aspx)
* [SharePoint Online: PowerShell to delete unique permissions in all list items](https://social.technet.microsoft.com/wiki/contents/articles/29718.sharepoint-online-powershell-to-delete-unique-permissions-in-all-list-items.aspx)
* [SharePoint Online: Remove users from site groups using PowerShell](https://learn.microsoft.com/en-us/archive/technet-wiki/37480.sharepoint-online-remove-users-from-site-groups-using-powershell)
* [Manage SharePoint Online Access Requests using Powershell](https://learn.microsoft.com/en-us/archive/technet-wiki/31157.manage-sharepoint-online-access-requests-using-powershell)
* [SharePoint 2013/2016: Approve or decline Access Requests using Powershell and CSOM](https://learn.microsoft.com/en-us/archive/technet-wiki/37401.sharepoint-20132016-approve-or-decline-access-requests-using-powershell-and-csom)



<br/><br/>
<h3>OneDrive for Business</h3>

* [PowerShell: OneDrive for Business usage report](https://learn.microsoft.com/en-us/archive/technet-wiki/31962.onedrive-for-business-usage-report-using-powershell)
* [OneDrive for Business notifications with Powershell](https://learn.microsoft.com/en-us/archive/technet-wiki/39385.onedrive-for-business-notifications-with-powershell)
* [OneDrive for Business sharing settings with PowerShell](https://learn.microsoft.com/en-us/archive/technet-wiki/39497.onedrive-for-business-sharing-settings-with-powershell)


<br/><br/>
<h3>Other</h3>

* [Get all checked-out files using Powershell](https://social.technet.microsoft.com/wiki/contents/articles/34215.sharepoint-online-get-all-checked-out-files-using-powershell.aspx)
* [SharePoint Online: Disable or enable attachments to list items using Powershell](https://learn.microsoft.com/en-us/archive/technet-wiki/30024.sharepoint-online-disable-or-enable-attachments-to-list-items-using-powershell)
* [SharePoint Online: Get any object with PowerShell ](http://social.technet.microsoft.com/wiki/contents/articles/37671.sharepoint-online-get-any-object-with-powershell-part-1.aspx)
* [Powershell GridView to help with SharePoint data viewing](https://learn.microsoft.com/en-us/archive/technet-wiki/34758.powershell-gridview-to-help-with-sharepoint-data-viewing)
* SharePoint 2016 Troubleshooting: Installation error - The tool was unable to install Web Server (IIS) Role
* Powershell in SharePoint: Disable comments on modern pages in entire site using CSOM


<br/><br/>
<h3>Office 365</h3>

* [Office 365 PowerShell Troubleshooting: quick guide](https://social.technet.microsoft.com/wiki/contents/articles/32035.office-365-powershell-troubleshooting-quick-guide.aspx)
* [Quick way to set up Office 365 tenant for testing](https://social.technet.microsoft.com/wiki/contents/articles/37130.quick-way-to-set-up-office-365-tenant-for-testing.aspx)
* [Customizing sign-in experience for external users](https://learn.microsoft.com/en-us/archive/technet-wiki/51868.office-365-customizing-sign-in-experience-for-external-users)
* Office 365 data loss protection: Prevent your SharePoint list from deletion using Powershell
* Exchange Online: What mailboxes User has access to?



