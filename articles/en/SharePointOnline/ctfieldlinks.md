---
layout: page
title: 'SharePoint content types - fieldlinks and fields'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2023-12-30'
---

SharePoint Online list columns are represented as either Field or FieldRef Element (ContentType) elements in the various SharePoint schemas, such as site, list, and content type definitions. The columns are represented as Field elements in site and list definitions. However, the column references are represented as FieldRef Element (ContentType) elements in content type definitions. In simple words SPFields are fields themselves, while SPFieldLinks are references to the fields.

It stems from the fact that Lists and Webs contain the actual fields with field data. A content type, on the other hand, only holds Field Reference, which simply points at the corresponding field in the list or web. As explained in MSDN:
 
The SPFieldCollection object provides developers a way to get a combined view of a column's attributes, as they exist in that content type. Each SPField object represents all the attributes of a column, or field, definition, merged with those attributes that have been overridden in the field reference for that content type. 
When you access a SPField in a content type, SharePoint Foundation merges the field definition with the field reference, and returns the resulting SPField object to you. This prevents developers from having to look up a field definition, and then look up all the attributes in the field definition that are overridden by the field reference for that content type.
Because of this, there is a 1-to-1 correlation between the items in the SPFieldLinkCollection and SPFieldCollection objects. For each SPFieldLink object that you add to a content type, SharePoint Foundation adds a corresponding SPField object that represents the combined view of that column as it is defined in the content type.




Comparison of Field Names and Field Link Names of the content types from a Content Type Hub where the names different.

| **ContentType**                 | **Field Title**                 | **FieldInternalName**       | **Field Link**            |
|---------------------------------|---------------------------------|-----------------------------|---------------------------|
| **Circulation**                 | Append-Only Comments            | V3Comments                  | Comments                  |
| **Phone Call Memo**             | Append-Only Comments            | V3Comments                  | Comments                  |
| **Display Template**            | Comments                        | Comments                    | Description               |
| **Display Template Code**       | Comments                        | Comments                    | Description               |
| **JavaScript Display Template** | Comments                        | Comments                    | Description               |
| **InfoPath Form Template**      | Content Type ID                 | CustomContentTypeId         | CusomContentTypeId        |
| **User Workflow Document**      | Visibility                      | NoCodeVisibility            | Visibility                |
| **Event**                       | Start Time                      | StartDate                   | EventDate                 |
| **Event**                       | Description                     | Comments                    | Description               |
| **Reservations**                | Start Time                      | StartDate                   | EventDate                 |
| **Reservations**                | Description                     | Comments                    | Description               |
| **Schedule and Reservations**   | Start Time                      | StartDate                   | EventDate                 |
| **Schedule and Reservations**   | Description                     | Comments                    | Description               |
| **Schedule**                    | Start Time                      | StartDate                   | EventDate                 |
| **Schedule**                    | Description                     | Comments                    | Description               |
| **Issue**                       | Due Date                        | TaskDueDate                 | DueDate                   |
| **Message**                     | Discussion Title                | DiscussionTitleLookup       | DiscussionSubjectLookup   |
| **Message**                     | Shortest Thread-Index Id Lookup | ShortestThreadIndexIdLookup | ShortestThreadIndexLookup |
| **Task**                        | Due Date                        | TaskDueDate                 | DueDate                   |
| **Task**                            | Task Status                     | TaskStatus                  | Status                    |
| **Workflow Task (SharePoint 2013)** | Due Date                        | TaskDueDate                 | DueDate                   |
| **Workflow Task (SharePoint 2013)** | Task Status                     | TaskStatus                  | Status                    |
| **Workflow Task (SharePoint 2013)** | Instance Id                     | WF4InstanceId               | WorkflowInstanceId        |
| **Workflow Task**                   | Due Date                        | TaskDueDate                 | DueDate                   |
| **Workflow Task**                   | Task Status                     | TaskStatus                  | Status                    |
| **Workflow Task**                   | Related Content                 | WorkflowLink                | Link                      |
| **SharePoint Server Workflow Task** | Due Date                        | TaskDueDate                 | DueDate                   |
| **SharePoint Server Workflow Task** | Task Status                     | TaskStatus                  | Status                    |
| **SharePoint Server Workflow Task** | Related Content                 | WorkflowLink                | Link                      |
| **Administrative Task**             | Action                          | AdminTaskAction             | Action                    |
| **Administrative Task**             | Description                     | AdminTaskDescription        | Description               |
| **Administrative Task**             | Order                           | AdminTaskOrder              | Priority                  |
| **Administrative Task**             | Task Status                     | TaskStatus                  | Status                    |
| **Administrative Task**             | Due Date                        | TaskDueDate                 | DueDate                   |
| **Workflow History**                | Description                     | DLC_Description             | Description               |
| **Discussion**                      | Shortest Thread-Index Id Lookup | ShortestThreadIndexIdLookup | ShortestThreadIndexLookup |
| **Summary Task**                    | Task Status                     | TaskStatus                  | Status                    |
| **Summary Task**                    | Due Date                        | TaskDueDate                 | DueDate                   |
| **Document Set**                    |	Description	                    | DocumentSetDescription	     | Description               |





<!-- Default Statcounter code for CT-fieldlinks
https://powershellscripts.github.io/articles/en/SharePointOnline/SharePoint%20content%20types%20-%20
-->
<script type="text/javascript">
var sc_project=13025467; 
var sc_invisible=1; 
var sc_security="b39e5a70"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/13025467/0/b39e5a70/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->