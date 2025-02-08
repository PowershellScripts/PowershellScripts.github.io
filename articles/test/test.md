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


	<li style="--i: 4"><a href="https://powershellscripts.github.io/articles/en/PowerPlatform/sitepages/">
		<span style="display: block; text-align: right;">2024-01-05
		<h3>Site Pages library in Power Automate Flow</h3></span>
		<p>This article shows how to use Site Pages library in Power Automate flows.</p>
	</a></li>

	<li style="--i: 5"><a href="https://powershellscripts.github.io/articles/en/Other/mailboxes/">
		<span style="display: block; text-align: right;">2024-12-30
		<h3>Exchange Online: What mailboxes User has access to?</h3></span>
		<p>The article describes a way how to verify user’s access rights to other people’s mailboxes.</p>
	</a></li>


	<li style="--i: 6"><a href="https://powershellscripts.github.io/articles/en/Other/mailboxes/">
		<span style="display: block; text-align: right;">2024-12-30
		<h3>Exchange Online: What mailboxes User has access to?</h3></span>
		<p>The article describes a way how to verify user’s access rights to other people’s mailboxes.</p>
	</a></li>


	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/viewanalytics/">
		<span style="display: block; text-align: right;">2024-12-29
		<h3>Viva Engage: View Analytics</h3></span>
		<p>As a leader or a corporate communicator managing campaign you can view the audience summary of your campaign. Have a look at some of the cool options that Viva Engage is offering.</p>
	</a></li>

	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/spo/createdashboard/">
		<span style="display: block; text-align: right;">2024-11-30
		<h3>Create SharePoint dashboard</h3></span>
		<p>A dashboard in SharePoint is a page primarily designed for mobile use, providing users with quick access to essential tools and resources. Use PnP Powershell and choose Dashboard layout to create a dashboard.</p>
	</a></li>


	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/spo/comparetenants/">
		<span style="display: block; text-align: right;">2024-11-17
		<h3>Compare SharePoint tenant settings</h3></span>
		<p>The “Run As” feature in the Microsoft 365 admin center allows administrators to simulate and test various admin roles. It allows to test the permissions associated with every Microsoft 365 admin role.</p>
	</a></li>

	<li style="--i: 7"><a href="https://powershellscripts.github.io/articles/en/InformationProtection/runasadmin/">
		<span style="display: block; text-align: right;">2024-11-16
		<h3>Microsoft 365: Run As [Product] Admin</h3></span>
		<p>The “Run As” feature in the Microsoft 365 admin center allows administrators to simulate and test various admin roles. It allows to test the permissions associated with every Microsoft 365 admin role.</p>
	</a></li>


	<li style="--i: 7"><a href="https://powershellscripts.github.io/articles/en/InformationProtection/compareroles/">
		<span style="display: block; text-align: right;">2024-11-09
		<h3>Microsoft 365: compare admin role permissions</h3></span>
		<p>In Microsoft 365 admin center you can now compare various administrators’ roles and select the one with minimum privilege for your admin accounts. The comparison lists detailed permissions of each role, and can be used for Compliance or ISO Documentation.</p>
	</a></li>


	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/spo/hidebuttons/">
		<span style="display: block; text-align: right;">2024-11-09
		<h3>SharePoint list view: hide new button</h3></span>
		<p>This article will give you a few JSON formatting examples on how to hide buttons in the command bar of a SharePoint list view, such as “add new item”, or “automate”.</p>
	</a></li>


	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/removeresources/">
		<span style="display: block; text-align: right;">2024-11-03
		<h3>Remove Community Resources in Viva Engage</h3></span>
		<p>On the right-hand side of your Viva Engage Community you can see the available Resources such as SharePoint Site, Planner and OneNote. With the newest update from Microsoft you can now hide these Resources on the main page of the Viva Engage Community.</p>
	</a></li>

	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/MoveConversation/">
		<span style="display: block; text-align: right;">2024-10-03
		<h3>Move Viva Engage conversation to another community</h3></span>
		<p>You can move a Viva Engage conversation to another community or stop users from moving posts to your community.</p>
	</a></li>

	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/findctidPS/">
		<span style="display: block; text-align: right;">2024-08-18
		<h3>Find content type ID using Powershell</h3></span>
		<p>Content type IDs in SharePoint uniquely identify the content type. This article helps you find the ID using Powershell.</p>
	</a></li>

	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/findctid/">
		<span style="display: block; text-align: right;">2024-08-18
		<h3>How to find SharePoint Online content type ID?</h3></span>
		<p>Content type IDs in SharePoint uniquely identify the content type. This article helps you find the ID using User Interface.</p>
	</a></li>
 
	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/getversionhistory/">
		<span style="display: block; text-align: right;">2024-08-11
		<h3>Get version history programmatically</h3></span>
		<p>The following PnP Powershell cmdlets can help you retrieve the older versions for each SharePoint file.</p>
	</a></li>
	
	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/countfilesunique/">
		<span style="display: block; text-align: right;">2024-08-11
		<h3>Count files or items with unique permissions using Powershell</h3></span>
		<p>Using PnP Powershell you can programmatically count files in a folder or the entire library in SharePoint Online that have unique permissions.</p>
	</a></li>

		<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/countfiles/">
		<span style="display: block; text-align: right;">2024-08-10
		<h3>Count files in a folder using Powershell</h3></span>
		<p>Using PnP Powershell you can programmatically count files in a folder or the entire library.</p>
	</a></li>

	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/spo/enablect">
		<span style="display: block; text-align: right;">2024-08-03
		<h3>Update SharePoint list item without changing the modified date</h3></span>
		<p>If you want to use multiple content types in your SharePoint list, you need to enable content type management first. Here is how to do it using PnP POwershell, Powershell & CSOM or simply your browser.</p>
	</a></li>

	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/systemupdateitem/">
		<span style="display: block; text-align: right;">2024-08-03
		<h3>Update SharePoint list item without changing the modified date</h3></span>
		<p>Updating SharePoint list item without changing the modified date is often called a system-update. This article shows you how to do it using PnP Powershell.</p>
	</a></li>

	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/systemupdatefolder/">
		<span style="display: block; text-align: right;">2024-08-03
		<h3>Update SharePoint folder without creating a new version</h3></span>
		<p>How to update a SharePoint Online folder without creating a new version or triggering a workflow.</p>
	</a></li>

		<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/addpermgroup/">
		<span style="display: block; text-align: right;">2024-08-01
		<h3>Add SharePoint site permissions to a group using PnP</h3></span>
		<p>Using the PnP PowerShell module, you can assign permissions to an existing group within the Microsoft 365 environment. It can be a SharePoint group, security group or Microsoft 365 group. </p>
	</a></li>

	<li style="--i: 12"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/spoclaims/">
		<span style="display: block; text-align: right;">2024-08-01
		<h3>SharePoint Claims Deep Dive</h3></span>
		<p>In SharePoint Online, user and group identities are represented by different claim providers, which determine how identities are authenticated and identified within the system. SharePoint claim provider prefixes are used to format login names and indicate the type of authentication and the source of the identity claim.</p>
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
 

 	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/removeleader/">
		<span style="display: block; text-align: right;">2024-02-17
		<h3>Remove a Viva Engage leader</h3></span>		
		<p>The leadership feature in Viva allows you to identify the leaders in your organization and the leaders to reach their targeted audiences. When you want to remove the leader functionalities from a user, you need to remove him from the leaders list in the Viva Admin Center. </p>
	</a></li>

	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/How%20to%20post%20as%20delegate/">
		<span style="display: block; text-align: right;">2024-02-03
		<h3>Viva: How to post as a delegate</h3></span>		
		<p>Now, you can post on behalf of another person in Viva Engage. This feature allows Viva Engage users to assign a delegate who can post on behalf of them. Configure it through Yammer’s settings section... </p>
	</a></li>

	<li style="--i: 9"><a href="https://powershellscripts.github.io/articles/en/Viva/leadervscommunicator/">
		<span style="display: block; text-align: right;">2024-02-03
		<h3>Viva Engage: Leaders vs Corporate Communicators</h3></span>		
		<p>Viva Engage provides two special privileged roles that allow users for extra communication capabilities. One of them is Leaders and the other Corporate Communicator. The article compares the two roles. </p>
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





<br/><br/><br/><br/>
Links to my older Social Technet Articles:

<h3>Content Types</h3>

<li> [SharePoint Online content types in Powershell: Add](https://social.technet.microsoft.com/wiki/contents/articles/31051.sharepoint-online-content-types-in-powershell-add.aspx) </li>

* [SharePoint Online content types in Powershell: Get](http://social.technet.microsoft.com/wiki/contents/articles/31151.sharepoint-online-content-types-in-powershell-get.aspx)

* [SharePoint Online content types in Powershell: Edit](https://social.technet.microsoft.com/wiki/contents/articles/31444.sharepoint-online-content-types-in-powershell-edit.aspx)
* [SharePoint Online: Turn on support for multiple content types in a list or library using Powershell](https://social.technet.microsoft.com/wiki/contents/articles/30038.sharepoint-online-turn-on-support-for-multiple-content-types-in-a-list-or-library-using-powershell.aspx)
* [SharePoint Online content types in PowerShell: Group property](http://social.technet.microsoft.com/wiki/contents/articles/31725.sharepoint-online-content-types-in-powershell-group-property.aspx)
* [SharePoint Online: Remove site content type using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/30158.sharepoint-online-remove-site-content-type-using-powershell.aspx)
* [SharePoint Online content types in Powershell: Fields vs Field Links](https://social.technet.microsoft.com/wiki/contents/articles/31694.sharepoint-online-content-types-in-powershell-fields-vs-field-links.aspx)
* [SharePoint Online content types in Powershell: Known Errors](https://social.technet.microsoft.com/wiki/contents/articles/31770.sharepoint-online-content-types-in-powershell-known-errors.aspx)
* [Find content type ID in SharePoint Online using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/35717.find-content-type-id-in-sharepoint-online-using-powershell.aspx)
* [Content Type is still in use: Powershell to remove items and content types](https://social.technet.microsoft.com/wiki/contents/articles/35716.content-type-is-still-in-use-powershell-to-remove-items-and-content-types.aspx)

<br/><br/>
<h3>Versioning</h3>

* [Versioning and SharePoint: the Powershell perspective (part 1)](http://social.technet.microsoft.com/wiki/contents/articles/30115.versioning-and-sharepoint-the-powershell-perspective-part-1.aspx)

* [Versioning and SharePoint: the Powershell perspective (part 2)](https://social.technet.microsoft.com/wiki/contents/articles/30150.versioning-and-sharepoint-the-powershell-perspective-part-2.aspx)
