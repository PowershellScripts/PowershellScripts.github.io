
# Intro

In SharePoint Search, especially when using the PnP (Patterns and Practices) Search Web Part, you can use the KQL (Keyword Query Language) query template to build dynamic queries. The query template allows for conditional logic using tokens and variables, but it's not as straightforward as using traditional programming "if" statements. Instead, you can achieve conditional logic by leveraging tokens like `{?{ }}` to include or exclude parts of the query based on conditions.

Here are a few examples of how you can use conditional logic in a query template.

### Example Scenario:
You want to modify the search query based on whether a user has provided a specific filter or not.

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


By using the `{?{ }}` syntax in your PnP Search Web Part query template, you can introduce dynamic, conditional logic similar to "if" statements in programming.




### Example 1: Search for Documents Modified in the Last 30 Days
```kql
{searchTerms} AND (FileExtension:docx OR FileExtension:pdf) AND Modified:{Today-30}..{Today}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `FileExtension:docx OR FileExtension:pdf`: Filters to show only Word documents and PDFs.
- `Modified:{Today-30}..{Today}`: Filters to show documents modified in the last 30 days.

### Example 2: Search for Items by Current User
```kql
{searchTerms} AND Author:{User.Name}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Author:{User.Name}`: Filters to show items where the author is the current user.

### Example 3: Search for Tasks Due Today
```kql
ContentType:Task AND DueDate:{Today}
```
#### Explanation:
- `ContentType:Task`: Filters to show only items of content type Task.
- `DueDate:{Today}`: Filters to show tasks that are due today.

### Example 4: Search Within a Specific Site Collection
```kql
{searchTerms} AND Path:https://yoursitecollection.sharepoint.com/sites/specificsite
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Path:https://yoursitecollection.sharepoint.com/sites/specificsite`: Limits the search to items within a specific site collection.

### Example 5: Search with Query String Parameters for Category and Date Range
```kql
{searchTerms} {?{QueryString.Category}Category:{QueryString.Category}} {?{QueryString.StartDate}Modified:{QueryString.StartDate}..{QueryString.EndDate}}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Category:{QueryString.Category}`: Filters by a category passed in the query string.
- `Modified:{QueryString.StartDate}..{QueryString.EndDate}`: Filters by a date range passed in the query string.

### Example 6: Search for Content with Specific Metadata
```kql
{searchTerms} AND Department:Finance AND ContentType:Document
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Department:Finance`: Filters to show only items where the Department metadata is set to Finance.
- `ContentType:Document`: Filters to show only items of content type Document.

### Example 7: Search for Items Tagged with Specific Terms
```kql
{searchTerms} AND (Tag:ProjectX OR Tag:Important)
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `Tag:ProjectX OR Tag:Important`: Filters to show items tagged with either ProjectX or Important.

### Example 8: Search for Recently Viewed Items by the User
```kql
{searchTerms} AND ViewedBy:{User.Name} AND LastViewedTime:{Today-7}..{Today}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `ViewedBy:{User.Name}`: Filters to show items viewed by the current user.
- `LastViewedTime:{Today-7}..{Today}`: Filters to show items viewed in the last 7 days.

### Example 9: Search for Items Excluding a Specific Folder
```kql
{searchTerms} -Path:https://yoursitecollection.sharepoint.com/sites/specificsite/Shared%20Documents/ExcludeFolder/*
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `-Path:https://yoursitecollection.sharepoint.com/sites/specificsite/Shared%20Documents/ExcludeFolder/*`: Excludes items from a specific folder.

### Example 10: Search for Items Created by the Current User in the Last Year
```kql
{searchTerms} AND CreatedBy:{User.Name} AND Created:{Today-365}..{Today}
```
#### Explanation:
- `{searchTerms}`: Placeholder for the user's search terms.
- `CreatedBy:{User.Name}`: Filters to show items created by the current user.
- `Created:{Today-365}..{Today}`: Filters to show items created in the last year.



### Final Notes:
- Ensure that the tokens you use match the actual parameters or variables available in your context.
- Test your query templates thoroughly to confirm they behave as expected with various input conditions.
