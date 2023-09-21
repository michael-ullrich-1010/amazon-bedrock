#### AWS Config Rule Policies <a id="policy_type_configure_config"></a> [🔝](#top)

> Folder: [/10_configure/config_rules](/10_configure/config_rules)

This policies allow you to specify the provisioning of custom AWS Config Rule to your member accounts.
SEMPER uses boto3 [ConfigService.Client.put_config_rule](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/config.html#ConfigService.Client.put_config_rule) for this feature.

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "configure": {
    "policyScope": {...},
    "configRuleSettings": {
      "configRuleName": 'string',
      "configRuleDescription": 'string',
      "complianceResourceTypes": ['string'],
      "sourceOwner": "CUSTOM_LAMBDA" | "AWS",
      "sourceIdentifier": 'string',
      "sourceDetails": [
        {
          "messageType": 'string',
          "maximumExecutionFrequency": 'string'
        }
      ]
      "inputParameters ": 'string'
    }
  }
  ...
}
```

| Key                            | Value-Type   | Comment |
| :---                           | :---         | :---    |
| .policyScope                   | object       | (optional) as described in this chapter [Section policyScope](#policy_scope). |
| .configRuleSettings            | object       | specifying the attributes used for the boto3 call. |
| . .configRuleName              | string       | according to boto3 [ConfigRuleName](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/config.html#ConfigService.Client.put_config_rule)-specification - will be prefixed with ***semper-***.|
| . .configRuleDescription       | string       | (optional) according to boto3 [Description](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/config.html#ConfigService.Client.put_config_rule)-specification.      |
| . .complianceResourceTypes     | array of string |  (optional) according to boto3 [Scope.ComplianceResourceTypes](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/config.html#ConfigService.Client.put_config_rule)-specification |
| . .sourceOwner                 | string       | Either "CUSTOM_LAMBDA" for a custom Lambda function in the Core Security account or "AWS" for AWS managed Config rules. |
| . .sourceIdentifier            | string       | If "sourceOwner" is "CUSTOM_LAMBDA", name of the custom evaluation Lambda hosted in the Core Security that is valid for this Config Rule. <br> If "sourceOwner" is "AWS", identifier of the AWS managed rule. List of [AWS Config Managed Rules](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html). E.g. *IAM_PASSWORD_POLICY* |
| . .sourceDetails               | list(object) | **_NOTE:_** Required only if "sourceOwner" is "CUSTOM_LAMBDA". |
| . . .messageType               | string       | According to [AWS Config Managed Rules](https://docs.aws.amazon.com/config/latest/APIReference/API_SourceDetail.html) API Reference |
| . . .maximumExecutionFrequency | string       | (Only required if messageType is 'ScheduledNotification') According to [AWS Config Managed Rules](https://docs.aws.amazon.com/config/latest/APIReference/API_SourceDetail.html) API Reference |
| . .inputParameters             | string       | (optional) A string, in JSON format, that is passed to the Config rule Lambda function. |

