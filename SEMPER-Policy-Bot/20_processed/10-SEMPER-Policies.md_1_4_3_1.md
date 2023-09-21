#### Use-Case SQS Fan-Out <a id="extension_sqs_fanout"></a> [🔝](#top)

The concept of this use-case is, that SQS URLs can be specified, where the processed Security Finding will be sent to.
The SQS Consumer (Lambda) will receive the full JSON of the processed Security Finding and can take the full context to determine further steps.
Examples are:

- Auto Remediation
- Notification of Workload-Owners
- Notification of Security-Team

>JSON-Key: "sqsFanOut"

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "extension": {
    ...
    "extensionBlock": {
      "sqsFanOut": [
        {
          "sqsName": "ar-close-sg-ports-trigger"
          ...
        },
        {
          "sqsUrl": "https://sqs.eu-central-1.amazonaws.com/*accountId*/notify-technical-account-owner-trigger"
          ...
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
| .extensionBlock | object | Security Findings that match the findingPattern will be extended by the extensionBlock-JSON. |
| ..sqsFanOut     | list   | The list of SQS Fan-Out specifications that will be executed in parallel. |
| ...sqsUrl       | string | (conflicts with sqsName) Security Finding will be sent to the specified SQS URL. |
| ...sqsName      | string | (conflicts with sqsURL) Security Finding will be sent to the specified SQS Name in the SEMPER region of the Core Security account. |
| ...Dynmamic Key | string | (optional) For the processor of the further context information can be provided. |

