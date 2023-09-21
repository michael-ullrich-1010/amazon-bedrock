#### Samples for "policyScope" <a id="policy_scope_samples"></a> [🠕](#top)

Sample 1: Exclude all regions and include "us-east-1" only.
Via **exclude** all regions will be ignored and via **forceInclude** one specific region will be put into scope again (take care that [SCPs](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html) might prevent resources in regions):

```json {linenos=table,hl_lines=[],linenostart=50}
Sample:
{
    ...
    "policyScope": {
      "regionScope": {
        "exclude": "*",
        "forceInclude": [
          "us-east-1"
        ]
      }
    }
    ...
}
```

Sample 2: Include all accounts that have account-tag "AccountName" starting with "Core".<br>
With th **exclude** section we ignore all possible account-contexts and with the **forceInclude** we include all accounts that have a tag-value prefixed with "Core" for the tag-key "AccountName".

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "policyScope": {
    "accountScope": {
      "exclude": "*",
      "forceInclude": {
        "accountTags": {
          "AccountName" : [
            {
              "prefix": "Core"
            }
          ]
        }
      }
    }
  }
  ...
}
```

Sample 3: Include all accounts that have account-tag "Environment" != "Production"

```json {linenos=table,hl_lines=[],linenostart=50}
This
{
  ...
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
  }
  ...
}
is the same like:
{
  ...
  "policyScope": {
    "accountScope": {
      "exclude": {
        "accountTags": {
          "Environment" : [
            "Production"
          ]
        }
      }
    }
  }
  ...
}
```

