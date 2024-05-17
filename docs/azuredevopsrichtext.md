# Get Azure DevOps Description / Rich Text Fields in PowerBI

Functions:

- [Work Item Description Function](#get-work-item-description-function)
- [Acceptance Criteria Function](#get-acceptance-criteria-function)
- [Latest Comment Function](#get-latest-comment)


## Get Work Item Description Function

```
let
    // Define the function with a parameter
    FetchWorkItemDescription = (workItemId as number) as text =>
        let
            // Fetch the JSON document using the dynamic URL
            GetDocument = Json.Document(VSTS.Contents(Text.Format("https://dev.azure.com/[Organization]/[Project]/_apis/wit/workitems/#{0}", {workItemId}))),
            // Convert to table
            ConvertToTable = Record.ToTable(GetDocument[fields]),
            // Transpose the table to switch rows and columns
            TransposedTable = Table.Transpose(ConvertToTable),
            // Promote the first row to headers
            PromotedHeaders = Table.PromoteHeaders(TransposedTable, [PromoteAllScalars=true]),
            // Attempt to fetch 'System.Description' and handle possible errors
            DescriptionResult = try PromotedHeaders[System.Description]{0} otherwise "",
            // Check for null or empty string to return 0 if true
            Description = if DescriptionResult = "" then "0" else DescriptionResult
        in
            Description
in
    FetchWorkItemDescription
```

## Get Acceptance Criteria Function

```
let
    // Define the function with a parameter
    FetchAcceptanceCriteria = (workItemId as number) as text =>
        let
            // Fetch the JSON document using the dynamic URL
            GetDocument = Json.Document(VSTS.Contents(Text.Format("https://dev.azure.com/[Organization]/[Project]/_apis/wit/workitems/#{0}", {workItemId}))),
            // Convert to table
            ConvertToTable = Record.ToTable(GetDocument[fields]),
            // Transpose the table to switch rows and columns
            TransposedTable = Table.Transpose(ConvertToTable),
            // Promote the first row to headers
            PromotedHeaders = Table.PromoteHeaders(TransposedTable, [PromoteAllScalars=true]),
            // Attempt to fetch 'Microsoft.VSTS.Common.AcceptanceCriteria' and handle possible errors
            AcceptanceCriteriaResult = try PromotedHeaders[Microsoft.VSTS.Common.AcceptanceCriteria]{0} otherwise "0"
        in
            AcceptanceCriteriaResult
in
    FetchAcceptanceCriteria
```

## Get Latest Comment
```
let
    // Define the function with a parameter
    FetchLatestComment = (workItemId as number) as text =>
        let
            // Fetch the JSON document using the comments URL
            GetComments = Json.Document(VSTS.Contents(Text.Format("https://dev.azure.com/[Organization]/[Project]/_apis/wit/workItems/#{0}/comments?orderBy=createdDate desc&$top=1", {workItemId}))),
            // Access the comments list from the correct field
            CommentsList = GetComments[comments],
            // Check if the comments list is empty
            LatestComment = if List.IsEmpty(CommentsList) then "No comments" else CommentsList{0}[text]
        in
            LatestComment
in
    FetchLatestComment
```