# Paying it Forward Repository

## Top 3 Power Automate Flows for Boosting producitivity in Azure DevOps

[![Watch the video](./)](https://youtu.be/cqinG_0JhKI?si=POmXHgo-a72wpdvF))


- Expression format - Markdown Snippet

```
concat(
  '- **Title:** ', items('Apply_to_each')['System.Title'], 
  '\n  - **Work Item Type:** ', items('Apply_to_each')['System.WorkItemType'], 
  '\n  - **State:** ', items('Apply_to_each')['System.State'], 
  '\n  - **Created Date:** ', formatDateTime(items('Apply_to_each')['System.CreatedDate'], 'yyyy-MM-dd'),
  '\n  - **Changed Date:** ', formatDateTime(items('Apply_to_each')['System.ChangedDate'], 'yyyy-MM-dd'), 
  '\n  - **URL:** [View Work Item]([Your Azure DevOps URL]', items('Apply_to_each')['System.Id'], ')\n\n'
)
```

Adaptive Card Manifest

``` json
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "TextBlock",
            "text": "Work Item Hygiene Report",
            "weight": "Bolder",
            "size": "Medium",
            "wrap": true
        },
        {
            "type": "TextBlock",
            "text": "The following work items haven't been updated for >5 days and are within the current sprint. Please update your work items!",
            "wrap": true
        },
        {
            "type": "TextBlock",
            "text": "@{variables('Work Item Title')}",
            "wrap": true,
            "markdown": true
        },
        {
            "type": "ActionSet",
            "actions": [
                {
                    "type": "Action.OpenUrl",
                    "title": "Update Now",
                    "url": "[Your Azure DevOps URL]"
                },
                {
                    "type": "Action.Submit",
                    "title": "Provide Feedback",
                    "data": {
                        "type": "feedback"
                    }
                },
                {
                    "type": "Action.ShowCard",
                    "title": "Documentation",
                    "card": {
                        "type": "AdaptiveCard",
                        "body": [
                            {
                                "type": "TextBlock",
                                "text": "Please review the documentation for updating work items:",
                                "wrap": true
                            }
                        ]
                    }
                }
            ]
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.3"
}
```
