---
layout: page
title: 'PnP Search query examples with KQL'
hero_image: '/img/IMG_20220521_140146.jpg'
menubar: docs_menu
show_sidebar: false
hero_height: is-small
date: '2024-07-14'
---

# Intro

KQL (Keyword Query Language) is a powerful syntax used primarily within Microsoft products like SharePoint and Microsoft Search to formulate search queries. Understanding its basic rules and operators such as `AND`, `OR`, and other principles is crucial for constructing effective search queries. Here’s a breakdown of these rules:

<br/>

## Boolean Operators:

- **AND**: The `AND` operator is used to narrow down search results by requiring that all specified conditions must be met. For example, `ContentType:Document AND Author:"John Doe"` retrieves documents authored by John Doe.

- **OR**: The `OR` operator broadens search results by retrieving items that match either of the specified conditions. For example, `ContentType:Document OR ContentType:Folder` retrieves both documents and folders.

- **NOT**: The `NOT` operator excludes items that match the specified condition. For example, `ContentType:Document NOT Author:"Jane Smith"` excludes documents authored by Jane Smith.

<br/>

## Grouping:

- Parentheses `()` can be used to group clauses together to control the logical order of operations. For example, `(ContentType:Document OR ContentType:Folder) AND Author:"John Doe"` retrieves documents or folders authored by John Doe.

<br/>

## Wildcards and Fuzziness:

- **Wildcards**: KQL supports `*` (asterisk) as a wildcard character to match any sequence of characters. For example, `Title:proj*` matches documents with titles starting with "proj".

- **Fuzziness**: KQL allows for fuzzy matching using the `~` (tilde) followed by a number to indicate how many edit distance operations (insertions, deletions, substitutions) are allowed. For example, `Title:proj~2` matches documents with titles similar to "proj" within an edit distance of 2.

<br/>

## Field Operators:

- **Field Specifiers**: Fields such as `Title:`, `Author:`, `ContentType:`, etc., specify which metadata or content property to search within. For example, `ContentType:Document` limits the search to documents only.

<br/>

## Range Queries:

- **Range Queries**: Using `..` (two dots) allows specifying a range for numerical or date fields. For example, `Modified:{Today-7}..{Today}` retrieves items modified within the last 7 days.

### Example Queries:

- **Simple Query**: `ContentType:Document AND Author:"John Doe"`
- **Complex Query**: `(ContentType:Document OR ContentType:Folder) AND (Author:"John Doe" OR Author:"Jane Smith")`
- **Wildcard Query**: `Title:proj*`
- **Fuzzy Query**: `Title:proj~2`
- **Range Query**: `Modified:{Today-30}..{Today}`

<br/>

## Usage Notes:

- **Syntax Sensitivity**: KQL syntax is case-insensitive but syntax structure (e.g., `AND`, `OR`) is crucial for correct query interpretation.
- **Managed Properties**: Ensure fields used in queries (`ContentType`, `Author`, etc.) are properly configured and indexed in your search system.
- **Testing**: Always test complex queries in a controlled environment to ensure they retrieve expected results.



<br/><br/><br/>

# Examples

<br/>


## * Search for Documents Modified in the Last 30 Days
```kql
{searchTerms} AND (FileExtension:docx OR FileExtension:pdf) AND Modified:{Today-30}..{Today}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `FileExtension:docx OR FileExtension:pdf`: Filters to show only Word documents and PDFs.
- `Modified:{Today-30}..{Today}`: Filters to show documents modified in the last 30 days.

<br/>

## * Search for Items Created by Current User
This shows items (SharePoint files) which the user has created.
```kql
{searchTerms} AND Author:{User.Name}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Author:{User.Name}`: Filters to show items where the author is the current user.

<br/>

## * Search for Items Modified by Current User
This shows items on which the user has worked.
```kql
{searchTerms} AND Editor:{User.Name}
```
#### Explanation:
- `{searchTerms}`: This placeholder is replaced by the user's search query.
- `Editor:{User.Name}`: This part of the query filters results to show only items where the Editor (the person who last modified the item) is the current user.

<br/>

##  * Search for Tasks Due Today
```kql
ContentType:Task AND DueDate:{Today}
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `DueDate:{Today}`: Filters to show tasks that are due today.

<br/>

## * Search for Tasks Due This Month in Specific Sites
```kql
ContentType:Task AND Path:https://yourtenant.sharepoint.com/sites/TEAMGR-* AND DueDate:{Today}..{EndOfMonth}
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `Path:https://yourtenant.sharepoint.com/sites/TEAMGR-*`: Limits the search to items within sites starting with TEAMGR.....
- `DueDate:{Today}..{EndOfMonth}`: Filters to show tasks that are due this month.

<br/>

## * Search for Overdue Tasks
```kql
ContentType:Task AND DueDate<={Today}
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `DueDate<={Today}`: Filters to show tasks where the due date is on or before today (i.e., overdue tasks).

<br/>

## * Search for Tasks Assigned to Users From a Specific Department
```kql
ContentType:Task AND Department:Dept2
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `Department:Dept2`: Filters to show tasks assigned to users whose department property is set to "Dept2". This requires the user profile properties to be indexed and searchable.

<br/>

## * Search for Tasks in Progress
```kql
ContentType:Task AND Status:"In Progress"
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `Status:"In Progress"`: Filters to show tasks that are currently in progress.

<br/>


## * Search Within a Specific SharePoint Site Collection
```kql
{searchTerms} AND Path:https://yoursitecollection.sharepoint.com/sites/specificsite
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Path:https://yoursitecollection.sharepoint.com/sites/specificsite`: Limits the search to items within a specific site collection.

<br/>

## * Search Within Multiple SharePoint Site Collections
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

## * Search Within Teams
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

## * Search Within All Teams
If all your team sites follow a naming convention such as TEAMGR-0001, TEAMGR-0002, etc., you can create a query in the PnP Search Web part that targets all sites following this pattern using a wildcard in the KQL query.
```kql
{searchTerms} AND Path:https://yourtenant.sharepoint.com/sites/TEAMGR-*
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- Path:https://yourtenant.sharepoint.com/sites/TEAMGR-*: Limits the search to items within any site that starts with TEAMGR- in your SharePoint tenant.




## * Search with Query String Parameters for Category and Date Range
```kql
{searchTerms} {?{QueryString.Category}Category:{QueryString.Category}} {?{QueryString.StartDate}Modified:{QueryString.StartDate}..{QueryString.EndDate}}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Category:{QueryString.Category}`: Filters by a category passed in the query string.
- `Modified:{QueryString.StartDate}..{QueryString.EndDate}`: Filters by a date range passed in the query string.

<br/>

## * Search for Content with Specific Metadata
```kql
{searchTerms} AND Department:Finance AND ContentType:Document
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Department:Finance`: Filters to show only items where the Department metadata is set to Finance.
- `ContentType:Document`: Filters to show only items of content type Document.

<br/>

## * Search for Items Tagged with Specific Terms
```kql
{searchTerms} AND (Tag:ProjectX OR Tag:Important)
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Tag:ProjectX OR Tag:Important`: Filters to show items tagged with either ProjectX or Important.

<br/>

## * Search for Recently Viewed Items by the User
```kql
{searchTerms} AND ViewedBy:{User.Name} AND LastViewedTime:{Today-7}..{Today}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `ViewedBy:{User.Name}`: Filters to show items viewed by the current user.
- `LastViewedTime:{Today-7}..{Today}`: Filters to show items viewed in the last 7 days.

<br/>

## * Search for Items Excluding a Specific Folder
```kql
{searchTerms} -Path:https://yoursitecollection.sharepoint.com/sites/specificsite/Shared%20Documents/ExcludeFolder/*
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `-Path:https://yoursitecollection.sharepoint.com/sites/specificsite/Shared%20Documents/ExcludeFolder/*`: Excludes items from a specific folder.

<br/>

## * Search for Items Created by the Current User in the Last Year
```kql
{searchTerms} AND CreatedBy:{User.Name} AND Created:{Today-365}..{Today}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `CreatedBy:{User.Name}`: Filters to show items created by the current user.
- `Created:{Today-365}..{Today}`: Filters to show items created in the last year.


<br/>

## * Search for Images in Team Sites
```kql
{searchTerms} AND Path:https://yourtenant.sharepoint.com/sites/TEAMGR-* AND (FileExtension:jpg OR FileExtension:png OR FileExtension:gif)
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Path:https://yourtenant.sharepoint.com/sites/TEAMGR-*`: Limits the search to items within any team site.
- `FileExtension:jpg OR FileExtension:png OR FileExtension:gif`: Filters to show only image files.

<br/>

## * Search for Videos in Team Sites
```kql
{searchTerms} AND Path:https://yourtenant.sharepoint.com/sites/TEAMGR-* AND (FileExtension:mp4 OR FileExtension:avi OR FileExtension:wmv)
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Path:https://yourtenant.sharepoint.com/sites/TEAMGR-*`: Limits the search to items within any team site.
- `FileExtension:mp4 OR FileExtension:avi OR FileExtension:wmv`: Filters to show only video files.

<br/>

## * Search for Videos Longer Than 5 Minutes
```kql
ContentType:Video AND DurationInSeconds>300
```
#### Explanation:
- `ContentType:Video`: Filters to show only items of content type Video.
- `DurationInSeconds>300`: Filters to show videos with a duration longer than 300 seconds (5 minutes).

<br/>

## * Search for Large Files
This query can help manage the site collection storage.
```kql
{searchTerms} AND Size>1073741824
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms. This can be omitted if you want to search for all large files without specific keywords.
- `Size>1073741824`: Filters to show items where the size is greater than 1GB (1GB = 1,073,741,824 bytes).


<br/>

## * Search for Suspiciously Small Files
This query can help find broken or empty files.
```kql
{searchTerms} AND Size<1000
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms. This can be omitted if you want to search for all large files without specific keywords.
- `Size<1000`: Filters to show items where the size is less than 1000 bytes.


<br/>

## * Search for Items with Comments
```kql
{searchTerms} AND IsDocument:true AND IsContainer:false AND Comments:{searchTerms}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `IsDocument:true AND IsContainer:false`: Filters to show only document items and exclude folders.
- `Comments:{searchTerms}`: Searches within the custom managed property Comments for the search terms.

<br/>

## * Search for Items with Comments
```kql
IsDocument:true AND IsContainer:false AND Comments:*
```
#### Explanation:
- `IsDocument:true AND IsContainer:false`: Filters to show only SharePoint document items and exclude folders.
- `Comments:*`: Filters to show documents where the Comments property is not empty.

<br/>

## * Search for Comments with a Specific Phrase
```kql
IsDocument:true AND IsContainer:false AND Comments:{searchTerms}
```
#### Explanation:
- `IsDocument:true AND IsContainer:false`: Filters to show only SharePoint document items and exclude folders.
- `Comments:{searchTerms}`: Searches within the custom managed property Comments for the search terms. Ensure that the Comments field is properly configured and indexed in SharePoint so that it can be searched effectively using KQL.

<br/>

## * Search for Folders and Document Sets
```kql
{searchTerms} AND IsContainer:true
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `IsContainer:true`: This query filters the search results to include only items that SharePoint identifies as containers. In SharePoint's context, containers typically refer to folders or document sets.


<br/>

## * Search for Items with High Importance
```kql
{searchTerms} AND Importance:High
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Importance:High`: Filters to show items marked with high importance.


<br/>

## * Search for Calendar Events
```kql
ContentType:Event AND EventDate:{Today}..{EndOfYear}
```
#### Explanation:
- `ContentType:Event`: Filters to show only calendar events.
- `EventDate:{Today}..{EndOfYear}`: Filters to show events occurring from today to the end of the year.


<br/>

## * Be More Specific 
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


# See Also

[Conditional query in PNP Search Webpart](https://powershellscripts.github.io/articles/en/SharePointOnline/pnpsearchqcond/)





<!-- Default Statcounter code for PnP Search Examples
https://powershellscripts.github.io/articles/en/SharePointOnline/pnpsearchqex/
-->
<script type="text/javascript">
var sc_project=13018560; 
var sc_invisible=0; 
var sc_security="979c5680"; 
var sc_client_storage="disabled"; 
var scJsHost = "https://";
document.write("<sc"+"ript type='text/javascript' src='" +
scJsHost+
"statcounter.com/counter/counter.js'></"+"script>");
</script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/13018560/0/979c5680/0/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->

