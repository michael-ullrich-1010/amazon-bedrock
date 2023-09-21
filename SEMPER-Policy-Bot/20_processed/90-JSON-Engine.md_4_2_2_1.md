#### AND / OR <a id="nuvibit_json_engine_examples_and_or"></a> [🔝](#top)

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "readOnly": false,
    "eventType": "AwsApiCall",
    "recipientAccountId": [
        "123456789012",
        "234567890123"
    ],
    "requestParameters": {
        "ipPermissions": {
            "items": {
                "ipProtocol": "tcp"
            }
        }
    }
}
```

```text
-> Pattern JSON will match to the Source JSON as all conditions are met.
```

