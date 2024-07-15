---
layout: page
title: 'Search query examples'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-07-14'
---

# Intro

In SharePoint Search, especially when using the PnP (Patterns and Practices) Search Web Part, you can use the KQL (Keyword Query Language) query template to build dynamic queries. The query template allows for conditional logic using tokens and variables, but it's not as straightforward as using traditional programming "if" statements. Instead, you can achieve conditional logic by leveraging tokens like `{?{ }}` to include or exclude parts of the query based on conditions. 

By using the `{?{ }}` syntax in your PnP Search Web Part query template, you can introduce dynamic, conditional logic similar to "if" statements in programming.

This article gives a few examples of how you can use conditional logic in a query template.

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


## <li> Search for Documents Modified in the Last 30 Days
```kql
{searchTerms} AND (FileExtension:docx OR FileExtension:pdf) AND Modified:{Today-30}..{Today}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `FileExtension:docx OR FileExtension:pdf`: Filters to show only Word documents and PDFs.
- `Modified:{Today-30}..{Today}`: Filters to show documents modified in the last 30 days.

<br/>

## <li> Search for Items Created by Current User
This shows items (SharePoint files) which the user has created.
```kql
{searchTerms} AND Author:{User.Name}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Author:{User.Name}`: Filters to show items where the author is the current user.

<br/>

## <li> Search for Items Modified by Current User
This shows items on which the user has worked.
```kql
{searchTerms} AND Editor:{User.Name}
```
#### Explanation:
- `{searchTerms}`: This placeholder is replaced by the user's search query.
- `Editor:{User.Name}`: This part of the query filters results to show only items where the Editor (the person who last modified the item) is the current user.

<br/>

## <li> Search for Tasks Due Today
```kql
ContentType:Task AND DueDate:{Today}
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `DueDate:{Today}`: Filters to show tasks that are due today.

<br/>

## <li> Search for Tasks Due This Month in Specific Sites
```kql
ContentType:Task AND Path:https://yourtenant.sharepoint.com/sites/TEAMGR-* AND DueDate:{Today}..{EndOfMonth}
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `Path:https://yourtenant.sharepoint.com/sites/TEAMGR-*`: Limits the search to items within sites starting with TEAMGR.....
- `DueDate:{Today}..{EndOfMonth}`: Filters to show tasks that are due this month.

<br/>

## <li> Search for Overdue Tasks
```kql
ContentType:Task AND DueDate<={Today}
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `DueDate<={Today}`: Filters to show tasks where the due date is on or before today (i.e., overdue tasks).

<br/>

## <li> Search for Tasks Assigned to Users From a Specific Department
```kql
ContentType:Task AND Department:Dept2
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `Department:Dept2`: Filters to show tasks assigned to users whose department property is set to "Dept2". This requires the user profile properties to be indexed and searchable.

<br/>

## <li> Search for Tasks in Progress
```kql
ContentType:Task AND Status:"In Progress"
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `Status:"In Progress"`: Filters to show tasks that are currently in progress.

<br/>


## <li> Search Within a Specific SharePoint Site Collection
```kql
{searchTerms} AND Path:https://yoursitecollection.sharepoint.com/sites/specificsite
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Path:https://yoursitecollection.sharepoint.com/sites/specificsite`: Limits the search to items within a specific site collection.

<br/>

## <li> Search Within Multiple SharePoint Site Collections
This query template ensures that the search is limited to items within any of the three specified SharePoint site collections.
```kql
{searchTerms} AND (Path:https://yoursitecollection.sharepoint.com/sites/site1 OR Path:https://yoursitecollection.sharepoint.com/sites/site2 OR Path:https://yoursitecollection.sharepoint.com/sites/site3)
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Path:https://yoursitecollection.sharepoint.com/sites/site1`: Limits the search to items within the first site collection.
- `Path:https://yoursitecollection.sharepoint.com/sites/site2`: Limits the search to items within the second site collection.
- `Path:https://yoursitecollection.sharepoint.com/sites/site3`: Limits the search to items within the third site collection.


<br/>

## <li> Search Within Teams
This query allows you to search for documents inside your Teams. Microsoft Teams stores its data in SharePoint (for files) and Exchange (for messages), so you can target the SharePoint sites associated with Teams to search for files and documents.
```kql
{searchTerms} AND (Path:https://yourtenant.sharepoint.com/sites/Team1 OR Path:https://yourtenant.sharepoint.com/sites/Team2 OR Path:https://yourtenant.sharepoint.com/sites/Team3)
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- Path:https://yourtenant.sharepoint.com/sites/Team1: Limits the search to items within the SharePoint site associated with Team1.
- Path:https://yourtenant.sharepoint.com/sites/Team2: Limits the search to items within the SharePoint site associated with Team2.
- Path:https://yourtenant.sharepoint.com/sites/Team3: Limits the search to items within the SharePoint site associated with Team3.

<br/>

## <li> Search Within All Teams
If all your team sites follow a naming convention such as TEAMGR-0001, TEAMGR-0002, etc., you can create a query in the PnP Search Web part that targets all sites following this pattern using a wildcard in the KQL query.
```kql
{searchTerms} AND Path:https://yourtenant.sharepoint.com/sites/TEAMGR-*
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- Path:https://yourtenant.sharepoint.com/sites/TEAMGR-*: Limits the search to items within any site that starts with TEAMGR- in your SharePoint tenant.




## <li> Search with Query String Parameters for Category and Date Range
```kql
{searchTerms} {?{QueryString.Category}Category:{QueryString.Category}} {?{QueryString.StartDate}Modified:{QueryString.StartDate}..{QueryString.EndDate}}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Category:{QueryString.Category}`: Filters by a category passed in the query string.
- `Modified:{QueryString.StartDate}..{QueryString.EndDate}`: Filters by a date range passed in the query string.

<br/>

## <li> Search for Content with Specific Metadata
```kql
{searchTerms} AND Department:Finance AND ContentType:Document
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Department:Finance`: Filters to show only items where the Department metadata is set to Finance.
- `ContentType:Document`: Filters to show only items of content type Document.

<br/>

## <li> Search for Items Tagged with Specific Terms
```kql
{searchTerms} AND (Tag:ProjectX OR Tag:Important)
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Tag:ProjectX OR Tag:Important`: Filters to show items tagged with either ProjectX or Important.

<br/>

## <li> Search for Recently Viewed Items by the User
```kql
{searchTerms} AND ViewedBy:{User.Name} AND LastViewedTime:{Today-7}..{Today}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `ViewedBy:{User.Name}`: Filters to show items viewed by the current user.
- `LastViewedTime:{Today-7}..{Today}`: Filters to show items viewed in the last 7 days.

<br/>

## <li> Search for Items Excluding a Specific Folder
```kql
{searchTerms} -Path:https://yoursitecollection.sharepoint.com/sites/specificsite/Shared%20Documents/ExcludeFolder/*
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `-Path:https://yoursitecollection.sharepoint.com/sites/specificsite/Shared%20Documents/ExcludeFolder/*`: Excludes items from a specific folder.

<br/>

## <li> Search for Items Created by the Current User in the Last Year
```kql
{searchTerms} AND CreatedBy:{User.Name} AND Created:{Today-365}..{Today}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `CreatedBy:{User.Name}`: Filters to show items created by the current user.
- `Created:{Today-365}..{Today}`: Filters to show items created in the last year.


<br/>

## <li> Search for Images in Team Sites
```kql
{searchTerms} AND Path:https://yourtenant.sharepoint.com/sites/TEAMGR-* AND (FileExtension:jpg OR FileExtension:png OR FileExtension:gif)
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Path:https://yourtenant.sharepoint.com/sites/TEAMGR-*`: Limits the search to items within any team site.
- `FileExtension:jpg OR FileExtension:png OR FileExtension:gif`: Filters to show only image files.

<br/>

## <li> Search for Videos in Team Sites
```kql
{searchTerms} AND Path:https://yourtenant.sharepoint.com/sites/TEAMGR-* AND (FileExtension:mp4 OR FileExtension:avi OR FileExtension:wmv)
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Path:https://yourtenant.sharepoint.com/sites/TEAMGR-*`: Limits the search to items within any team site.
- `FileExtension:mp4 OR FileExtension:avi OR FileExtension:wmv`: Filters to show only video files.

<br/>

## <li> Search for Videos Longer Than 5 Minutes
```kql
ContentType:Video AND DurationInSeconds>300
```
#### Explanation:
- `ContentType:Video`: Filters to show only items of content type Video.
- `DurationInSeconds>300`: Filters to show videos with a duration longer than 300 seconds (5 minutes).

<br/>

## <li> Search for Large Files
This query can help manage the site collection storage.
```kql
{searchTerms} AND Size>1073741824
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms. This can be omitted if you want to search for all large files without specific keywords.
- `Size>1073741824`: Filters to show items where the size is greater than 1GB (1GB = 1,073,741,824 bytes).


<br/>

## <li> Search for Suspiciously Small Files
This query can help find broken or empty files.
```kql
{searchTerms} AND Size<1000
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms. This can be omitted if you want to search for all large files without specific keywords.
- `Size<1000`: Filters to show items where the size is less than 1000 bytes.


<br/>

## <li> Search for Items with Comments
```kql
{searchTerms} AND IsDocument:true AND IsContainer:false AND Comments:{searchTerms}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `IsDocument:true AND IsContainer:false`: Filters to show only document items and exclude folders.
- `Comments:{searchTerms}`: Searches within the custom managed property Comments for the search terms.

<br/>

## <li> Search for Items with Comments
```kql
IsDocument:true AND IsContainer:false AND Comments:*
```
#### Explanation:
- `IsDocument:true AND IsContainer:false`: Filters to show only SharePoint document items and exclude folders.
- `Comments:*`: Filters to show documents where the Comments property is not empty.

<br/>

## <li> Search for Comments with a Specific Phrase
```kql
IsDocument:true AND IsContainer:false AND Comments:{searchTerms}
```
#### Explanation:
- `IsDocument:true AND IsContainer:false`: Filters to show only SharePoint document items and exclude folders.
- `Comments:{searchTerms}`: Searches within the custom managed property Comments for the search terms. Ensure that the Comments field is properly configured and indexed in SharePoint so that it can be searched effectively using KQL.

<br/>

## <li> Search for Folders and Document Sets
```kql
{searchTerms} AND IsContainer:true
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `IsContainer:true`: This query filters the search results to include only items that SharePoint identifies as containers. In SharePoint's context, containers typically refer to folders or document sets.


<br/>

## <li> Search for Items with High Importance
```kql
{searchTerms} AND Importance:High
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Importance:High`: Filters to show items marked with high importance.


<br/>

## <li> Search for Calendar Events
```kql
ContentType:Event AND EventDate:{Today}..{EndOfYear}
```
#### Explanation:
- `ContentType:Event`: Filters to show only calendar events.
- `EventDate:{Today}..{EndOfYear}`: Filters to show events occurring from today to the end of the year.


<br/>

## <li> Be More Specific 
Do not hesitate to combine the queries above.
```kql
(Path:https://yourtenant.sharepoint.com/sites/department1 OR Path:https://yourtenant.sharepoint.com/sites/department2) 
AND ContentType:Document 
AND Author:"Arleta Wanat" 
AND (FileType:docx OR FileType:xlsx OR FileType:pptx) 
AND Created>{Today-365} 
AND (Modified:{Today-7}..{Today}) 
AND Size>{1024} 
AND (Keywords:"project update" OR Keywords:"report") 
AND IsDocument:true 
AND IsContainer:false
```


### Final Notes:
- Ensure that the tokens you use match the actual parameters or variables available in your context.
- Test your query templates thoroughly to confirm they behave as expected with various input conditions.
