#### Use-Case Send to AWS Security Hub<a id="extension_securityhub"></a> [🔝](#top)

By specifying this extension block, you can create a Security Hub Finding based on the processed SEMPER Security Finding.

>JSON-Key: "sendToSecurityHub"

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "extension": {
    ...
    "extensionBlock": {
      "sendToSecurityHub": {
        "findingTitle": "SEMPER Finding Title",
        "findingSevertiy": 20,
        "findingComplianceStatus" : "FAILED",
        "findingResource" : {
          "id": "raw.detail.requestParameters.groupId",
          "type": "AwsEc2SecurityGroup"
        }
      }        
    }
  }
  ...
}
```

| Key                   | Value-Type | Comment |
| :---                  | :---  | :---  |
| extension             | object | |
| .extensionBlock       | object | Security Findings that match the findingPattern will be extended by the extensionBlock-JSON. |
| ..sendToSecurityHub   | object | |
| ...findingTitle       | string | (optional, default = 'SEMPER Finding') |
| ...findingDescription | string | (optional) |
| ...findingSevertiy    | number | (optional, default = 20) according to [AWS documentation](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_Severity.html). |
| ...findingComplianceStatus     | string | (optional, default = FAILED) according to [AWS documentation](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_Compliance.html#securityhub-Type-Compliance-Status). |
| ...findingResource    | object | (optional) |
| ....id                | string | (optional, default = '') you can self-reference the JSON of the processed Security Finding. <br> Example: 'raw.detail.requestParameters.keyId' |
| ....type              | string | (optional, default = 'AwsAccount') according to [AWS documentation](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_ResourceDetails.html). |

