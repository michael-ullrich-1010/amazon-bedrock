#### AWS EventBridge Rule Policies <a id="policy_type_configure_eventbridge"></a> [🔝](#top)

> Folder: [/10_configure/event_rules](/10_configure/event_rules)

This policies allow you to provision custom AWS EventBridge Rules to your member accounts.
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

