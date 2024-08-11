---
layout: page
title: 'Conditional query in PNP Search Webpart'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-07-14'
---

# Intro

In SharePoint Search, especially when using the PnP (Patterns and Practices) Search Web Part, you can use the KQL (Keyword Query Language) search query template to build dynamic queries. The search query template allows for conditional logic using tokens and variables, but it's not as straightforward as using traditional programming "if" statements. Instead, you can achieve conditional logic by leveraging tokens like `{?{ }}` to include or exclude parts of the search query based on conditions. 

By using the `{?{ }}` syntax in your PnP Search Web Part query template, you can introduce dynamic, conditional logic similar to "if" statements in programming.

This article gives a few examples of how you can use conditional logic in a search query template.

<br/><br/><br/>

# Detailed explanation
Imagine, you want to modify the PnP webpart search query based on whether a user has provided a specific filter or not.

### Step-by-Step Process:

1. **Open the PnP Search Web Part**:
   - Go to the SharePoint page where you want to configure the web part.
   - Edit the page and add or configure the "PnP - Search Results" web part.

2. **Edit the Query Template**:
   - In the web part properties, locate the "Search Query" or "Query Template" field.

3. **Use Conditional Logic in KQL**:
   - Use the `{?{ }}` syntax to conditionally include parts of the query based on the presence of a token or variable.

### Sample Query Template:
```kql
{searchTerms} {?{QueryString.FilterCategory}Category:{QueryString.FilterCategory}}
```

#### Explanation:
- `{searchTerms}`: This placeholder is replaced by the user's search query.
- `{?{QueryString.FilterCategory}Category:{QueryString.FilterCategory}}`: This conditional logic checks if `QueryString.FilterCategory` is present. If it is, it appends `Category:{QueryString.FilterCategory}` to the query.

### Detailed Breakdown:
- `QueryString.FilterCategory`: This is a token that gets replaced by the value of a query string parameter named `FilterCategory` if it exists.
- `Category:{QueryString.FilterCategory}`: This part of the query is added only if `QueryString.FilterCategory` is present.

### Additional Example with Multiple Conditions:
If you have multiple conditions, you can chain them using similar syntax:

```kql
{searchTerms} {?{QueryString.FilterCategory}Category:{QueryString.FilterCategory}} {?{QueryString.FilterDate}Date:{QueryString.FilterDate}}
```

### Explanation:
- This query template checks for both `FilterCategory` and `FilterDate` query string parameters.
- It appends the respective conditions to the search query if the parameters are present.

### Applying Tokens and Variables:
Tokens and variables can be retrieved from:
- Query string parameters
- Current user's properties
- Page properties

### Using Tokens:
Here are some common tokens you might use in a query template:
- `{searchTerms}`: The search terms entered by the user.
- `{User.Name}`: The current user's name.
- `{User.Email}`: The current user's email.
- `{Page.Title}`: The title of the current page.
- `{Today}`: The current date.

### Putting It All Together:
Here’s how a more complex query template with multiple conditions might look:

```kql
{searchTerms} {?{QueryString.FilterCategory}Category:{QueryString.FilterCategory}} {?{QueryString.FilterDate}Date:{QueryString.FilterDate}} {?{User.Name}Author:{User.Name}}
```

<br/><br/><br/>


# Examples

<br/>


## * Conditional Query Based on Content Type

```kql
{?{QueryString.ContentType} ContentType:{QueryString.ContentType}}
```

#### Explanation:
- `{?{QueryString.ContentType}`: Checks if the `ContentType` parameter exists in the query string.
- `ContentType:{QueryString.ContentType}`: If `ContentType` exists in the query string, it filters by that content type; otherwise, it doesn't apply any additional content type filter.

<br/>

## * Conditional Query Based on Modified Date Range

```kql
{?{QueryString.StartDate} Modified>={QueryString.StartDate} AND Modified<={QueryString.EndDate}}
```

#### Explanation:
- `{?{QueryString.StartDate}`: Checks if the `StartDate` parameter exists in the query string.
- `Modified>={QueryString.StartDate} AND Modified<={QueryString.EndDate}`: If `StartDate` exists in the query string, it filters items modified within the specified date range (`StartDate` to `EndDate`); otherwise, it doesn't apply any date range filter.

<br/>

## * Conditional Query Based on Author

```kql
{?{User.Name} Author:{User.Name}}
```

#### Explanation:
- `{?{User.Name}`: Checks if the current user (`User.Name`) is defined.
- `Author:{User.Name}`: If the current user is defined, it filters items authored by the current user; otherwise, it doesn't apply any author filter.

<br/>

## * Conditional Query Based on File Type

```kql
{?{QueryString.FileType} FileType:{QueryString.FileType}}
```

#### Explanation:
- `{?{QueryString.FileType}`: Checks if the `FileType` parameter exists in the query string.
- `FileType:{QueryString.FileType}`: If `FileType` exists in the query string, it filters by that file type; otherwise, it doesn't apply any additional file type filter.



Using SharePoint hub ID in a conditional query typically involves filtering content based on the association of sites with a specific hub. Here's how you can structure a conditional query using SharePoint hub ID:

<br/>

## * Conditional Query Based on SharePoint Hub ID

```kql
{?{Site.HubId} Path:"https://yourtenant.sharepoint.com/sites/{Site.HubId}/" OR Path:"https://yourtenant.sharepoint.com/sites/anothersite"}
```

#### Explanation:
- `{?{Site.HubId}`: Checks if the current site belongs to a specific SharePoint hub.
- `Path:"https://yourtenant.sharepoint.com/sites/{Site.HubId}/"`: If the current site belongs to a hub (`Site.HubId` is defined), it filters items within the site associated with that hub; otherwise, it includes items from another predefined site.
- `Path:"https://yourtenant.sharepoint.com/sites/anothersite"`: This path represents an alternative site to search if the current site is not part of any hub or `Site.HubId` is not defined.

### Usage Notes:
- Ensure that `Site.HubId` is a valid and accessible property in your SharePoint environment.
- Replace `https://yourtenant.sharepoint.com/sites/{Site.HubId}/` with the actual URL structure used by your SharePoint sites.
- Adjust the alternative path (`Path:"https://yourtenant.sharepoint.com/sites/anothersite"`) to match the fallback or default site you want to include in case the hub association isn't applicable.

This query structure allows you to dynamically adjust search queries based on the SharePoint hub association (`Site.HubId`), providing flexibility in content filtering within your SharePoint environment. Adjust the paths and conditions as per your specific SharePoint setup and requirements.



# Final Notes:
- Ensure that the tokens you use match the actual parameters or variables available in your context.
- Test your query templates thoroughly to confirm they behave as expected with various input conditions.


# See Also
[PnP Search query examples with KQL](https://powershellscripts.github.io/articles/en/SharePointOnline/pnpsearchqex/)


<!-- Default Statcounter code for PnP Search conditional
https://powershellscripts.github.io/articles/en/SharePointOnline/pnpsearchqcond/
-->
<script type="text/javascript">
var sc_project=13018562; 
var sc_invisible=0; 
var sc_security="ef85d292"; 
var sc_client_storage="disabled"; 
var scJsHost = "https://";
document.write("<sc"+"ript type='text/javascript' src='" +
scJsHost+
"statcounter.com/counter/counter.js'></"+"script>");
</script>
<noscript><div class="statcounter"><a title="free web stats"
href="https://statcounter.com/" target="_blank"><img
class="statcounter"
src="https://c.statcounter.com/13018562/0/ef85d292/0/"
alt="free web stats"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
