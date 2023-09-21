### Filter-Policies <a id="policy_type_filter"></a> [🔝](#top)

SEMPER will aggregate all the security events from the provisioned SEMPER AWS Config Rules, the AWS EventBridge Rules (API calls via CloudTrail) and also the AWS Security Hub- and Amazon GuardDuty findings. SEMPER forwards all those security findings to a SEMPER Processing Lambda in your Core Security account.

This SEMPER Processing Lambda will determine the account-context (OU-ID, AWS account tags) of the originating account based on the account ID.

Depending on the source (AWS Event, AWS Security Hub or Amazon GuardDuty) SEMPER will iterate through all filtering policies in the following folders:
>Folder: /20_filtering/cloudtrail_api_calls
>Folder: /20_filtering/guardduty_findings
>Folder: /20_filtering/securityhub_findings

Based on the defined filter policies in those folders the processing Lambda will filter the the security findings

The generic structure of a SEMPER Filter-Policy looks like this:

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "filtering": {
    "policyScope": {...},
    "findingPattern": {...}
  }
  ...
}
```

| Key             | Value-Type | Comment |
| :---            | :---  | :---  |
| filtering       | object | |
| .policyScope    | object | (optional) Each Security Finding has an originating account and an AWS region of the emitting resource. <br>As described in this chapter [Section policyScope](#policy_scope) you can limit the application of a SEMPER policy to a specific account- and region-context. |
| .findingPattern | object | According to the [SEMPER Policy Syntax](#policy_syntax). |

