#### AWS Security Hub Configuration Policies <a id="policy_type_configure_securityhub"></a> [🔝](#top)

For AWS Security Hub please consider this diagram:

![aws-security-hub-model](docs/aws-security-hub-model.png)

For AWS Security Hub tailoring we have two types of policies - a policy for tailoring the standards and a policy-folder for tailoring the controls:
> Policy: [/10_configure/securityhub_standards.json](/10_configure/securityhub_standards.json)
> Folder: [/10_configure/securityhub_controls/](/10_configure/securityhub_controls)

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "configure": {
    "securityHubStandards": [
      {
        "standardName": "AWS Foundational Security Best Practices",
        "standardIdentifier": "aws-foundational-security-best-practices/v/1.0.0",
        "standardArn": "arn:aws:securityhub:${region}::standards/aws-foundational-security-best-practices/v/1.0.0",
        "targetState": "ENABLED",
        "policyScope": {}
      },
      ...
    ]
  }    
  ...
}
```

| Key                 | Value-Type | Comment |
| :---                | :---  | :---  |
| .policyScope        | object | (optional) as described in this chapter [Section policyScope](#policy_scope). |
| .eventSettings      | object | specifying the attributes used for the boto3 call. |
| . .eventName        | string | according to boto3 [Name](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule)-specification - will be prefixed with "semper-".|
| . .eventDescription | string | according to boto3 [Description](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule)-specification. |
| . .eventPattern     | array of string | The eventPattern has to follow this AWS specification: [Amazon EventBridge event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html) |

This policies allow you to enable AWS Security Hub  custom AWS EventBridge Rules to your member accounts.
SEMPER uses boto3 [Events.Client.put_rule](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule) for this feature.

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "configure": {
    "policyScope": {...},
    "eventSettings": {
      "eventName": 'string' according to boto3 Name-specification - will be prefixed with "semper-",
      "eventDescription": 'string' according to boto3 Description-specification,
      "eventPattern": 'string' following the specification described in the link below
    }
  }
  ...
}
```

| Key                 | Value-Type | Comment |
| :---                | :---  | :---  |
| .policyScope        | object | (optional) as described in this chapter [Section policyScope](#policy_scope). |
| .eventSettings      | object | specifying the attributes used for the boto3 call. |
| . .eventName        | string | according to boto3 [Name](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule)-specification - will be prefixed with "semper-".|
| . .eventDescription | string | according to boto3 [Description](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule)-specification. |
| . .eventPattern     | array of string | The eventPattern has to follow this AWS specification: [Amazon EventBridge event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html) |

