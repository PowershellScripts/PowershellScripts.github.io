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
	--c1: hsl(var(--h), 71%, var(--l));
	--c2: hsl(var(--h), 71%, var(--l2));
	
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
	<li style="--i: 8"><a href="https://powershellscripts.github.io/articles/en/Viva/vivagraphapi/">
		<span style="display: block; text-align: right;">2024-06-02
		<h3>Manage Viva Engage with Graph API</h3></span>
		<p>Get, create and manage your Viva Engage communities using Graph API. Examples with Graph Explorer and explanation of the API. Read on... </p>
	</a></li>
	
	<li style="--i: 8"><a href="https://powershellscripts.github.io/articles/en/Viva/Closeconversation/">
		<span style="display: block; text-align: right;">2024-05-26
		<h3>Close conversations in Viva Engage</h3></span>
		<p>There is a variety of reasons why you should be closing Viva Engage conversations. A guide to keeping your Viva Engage community in orderly fashion</p>
	</a></li>

	<li style="--i: 5"><a href="https://powershellscripts.github.io/articles/en/SharePointOnline/HideTeamsPrompt">
		<span style="display: block; text-align: right;">2024-03-24
		<h3>Hide Teamify Prompt</h3></span>
		<p>In SharePoint, when a site is created without being associated with a Microsoft 365 group or a Microsoft Teams team, users typically encounter a teamify prompt displayed in the bottom left corner of the site interface. You can remove this teamify prompt using a PnP cmdlet... </p>
	</a></li>
 
	<li style="--i: 8"><a href="https://powershellscripts.github.io/articles/en/Viva/How%20to%20post%20as%20delegate/">
		<span style="display: block; text-align: right;">2024-02-03
		<h3>Viva: How to post as a delegate</h3></span>		
		<p>Now, you can post on behalf of another person in Viva Engage. This feature allows Viva Engage users to assign a delegate who can post on behalf of them. Configure it through Yammer’s settings section... </p>
	</a></li>
 
	<li style="--i: 4"><a href="https://powershellscripts.github.io/articles/en/Viva/Post%20as%20a%20leader%20to%20specific%20groups/">
		<span style="display: block; text-align: right;">2024-01-14
		<h3>Post as a leader to specific groups</h3></span>
		<p>The leadership feature in Viva allows you to identify the leaders in your organization and the leaders to reach their targeted audiences.</p>
	</a></li>
 
	<li style="--i: 4"><a href="https://powershellscripts.github.io/articles/en/Viva/Add%20Viva%20Engage%20to%20your%20SharePoint%20pages/">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>Add Viva Engage to your SharePoint pages</h3></span>
		<p>Viva Engage Conversations web part allows page viewers to participate in discussions without exiting the SharePoint environment. Use it to replace your old comments section with Viva Engage discussions.</p>
	</a></li>

	<li style="--i: 4"><a href="https://powershellscripts.github.io/articles/en/Viva/Post%20as%20a%20leader%20to%20specific%20groups/">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>[Add Viva Engage to your SharePoint pages](https://powershellscripts.github.io/articles/en/Viva/Closeconversation/)</h3></span>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Adipiscing diam donec adipiscing tristique risus.</p>
	</a></li>
 
	<li style="--i: 4"><a href="https://powershellscripts.github.io/articles/en/Viva/Post%20as%20a%20leader%20to%20specific%20groups/">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>[Add Viva Engage to your SharePoint pages](https://powershellscripts.github.io/articles/en/Viva/Closeconversation/)</h3></span>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Adipiscing diam donec adipiscing tristique risus.</p>
	</a></li>
 
	<li style="--i: 4"><a href="https://powershellscripts.github.io/articles/en/Viva/Post%20as%20a%20leader%20to%20specific%20groups/">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>[Add Viva Engage to your SharePoint pages](https://powershellscripts.github.io/articles/en/Viva/Closeconversation/)</h3></span>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Adipiscing diam donec adipiscing tristique risus.</p>
	</li>
		<li style="--i: 5">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>[Add Viva Engage to your SharePoint pages](https://powershellscripts.github.io/articles/en/Viva/Closeconversation/)</h3></span>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Adipiscing diam donec adipiscing tristique risus.</p>
	</li>
		<li style="--i: 5">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>[Add Viva Engage to your SharePoint pages](https://powershellscripts.github.io/articles/en/Viva/Closeconversation/)</h3></span>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Adipiscing diam donec adipiscing tristique risus.</p>
	</li>
		<li style="--i: 5">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>[Add Viva Engage to your SharePoint pages](https://powershellscripts.github.io/articles/en/Viva/Closeconversation/)</h3></span>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Adipiscing diam donec adipiscing tristique risus.</p>
	</li>
		<li style="--i: 5">
		<span style="display: block; text-align: right;">2023-11-04
		<h3>[Add Viva Engage to your SharePoint pages](https://powershellscripts.github.io/articles/en/Viva/Closeconversation/)</h3></span>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Adipiscing diam donec adipiscing tristique risus.</p>
	</li>
</ol>


