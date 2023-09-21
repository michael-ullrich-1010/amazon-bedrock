#### Samples of Filter-Policies <a id="filter_policy_samples"> [🔝](#top)

```json {linenos=table,hl_lines=[],linenostart=50}
{
  "metaData": {
    "domain": "filter",
    "type": "eventbridge_rule",
    "title": "Ignore KMS events for all environments except 'Production'",
    ...
  },
  "filtering": {
    "policyScope": {
      "accountScope": {
        "exclude": "*",
        "forceInclude": {
          "accountTags": {
            "Environment" : [
              {
                "anything-but": "Production"
              }
            ]
          }
        }
      }
    },
    "findingPattern": {
      "detail": {
        "eventSource": [
          "kms.amazonaws.com"
        ],
        "eventName": [
          "DisableKey",
          "ScheduleKeyDeletion"
        ]
      }
    }
  },
  ...
}
```

```json {linenos=table,hl_lines=[],linenostart=50}
{
  "metaData": {
    "domain": "filter",
    "type": "eventbridge_rule",
    "title": "Drop CIS_AWS_1.4 findings for IAM User foundation_checkpoint_reader",
    ...
  },
  "filtering": {
    "findingPattern": {
      "ProductFields": {
        "StandardsGuideArn": [
          {
            "prefix": "arn:aws:securityhub:::ruleset/cis-aws-foundations-benchmark"
          }
        ],
        "RuleId": [
          "1.4"
        ]
      },
      "Resources": {
        "Id": [
          {
            "suffix": ":user/checkpoint_api"
          }
        ]
      }
    }
  },
  ...
}
```

