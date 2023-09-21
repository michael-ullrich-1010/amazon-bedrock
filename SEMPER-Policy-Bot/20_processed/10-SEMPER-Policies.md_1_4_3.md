### Extension-Policies <a id="policy_type_extension"></a> [🔝](#top)

Extension policies allow you a policy based extension of the finding json for post-processing stages.

Extension policies will be processed after the Filtering-Step and are located in the followinf folder:
>Folder: /30_extension

The generic structure of a SEMPER Extension-Policy looks like this:

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "extension": {
    "policyScope": {...},
    "findingPattern": {...},
    "extensionBlock": {
      "useCaseIdentifier": [
        {

        }
      ]
    }
  }
  ...
}
```

| Key             | Value-Type | Comment |
| :---            | :---  | :---  |
| extension       | object | |
| .policyScope    | object | (optional) Each Security Finding has an originating account and an AWS region of the emitting resource. <br>As described in this chapter [Section policyScope](#policy_scope) you can limit the application of a SEMPER policy to a specific account- and region-context. |
| .findingPattern | object | According to the [SEMPER Policy Syntax](#policy_syntax). |
| .extensionBlock | object | Security Findings that match the findingPattern will be extended by the extensionBlock-JSON. |

