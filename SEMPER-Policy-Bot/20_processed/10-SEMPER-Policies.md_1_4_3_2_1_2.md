###### Sample 2 [🔝](#top)

```json {linenos=table,hl_lines=[],linenostart=50}
{
  "metaData": {
    "domain": "extension",
    "title": "AWS Account root-user was used",
    ...
  },
  "extension": {
    "findingPattern": {
      "detail-type": [
        "AWS API Call via CloudTrail",
        "AWS Console Sign In via CloudTrail"
      ],
      "detail": {
        "userIdentity": {
          "type": "Root"
        }
      }
    },
    "extensionBlock": {
      "sqsFanOut": [
        {
          "sqsName": "instant-alarm-trigger"
        }
      ],
      "sendToSecurityHub": {
        "findingTitle": "Usage of root-user",
        "findingSevertiy": 95,
        "findingComplianceStatus" : "FAILED",
      }        
    }
  }
}
```

