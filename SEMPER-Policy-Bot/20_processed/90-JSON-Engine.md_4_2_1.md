### Source JSON  <a id="nuvibit_json_engine_examples_source"></a> [🔝](#top)

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "sourceIPAddress": "87.200.73.179",
    "requestParameters": {
        "ipPermissions": {
            "items": [
                {
                    "ipProtocol": "tcp",
                    "fromPort": 70,
                    "toPort": 90,
                    "ipRanges": {
                        "items": [
                            {
                                "cidrIp": "0.0.0.0/16"
                            }
                        ]
                    }
                }
            ]
        }
    },
    "readOnly": false,
    "eventType": "AwsApiCall",
    "managementEvent": true,
    "recipientAccountId": "123456789012",
    "eventCategory": "Management",
    "sessionCredentialFromConsole": "true",
    "a-count": 5,
    "b-count": 5,
    "c-count": 0,
    "d-count": 10,
    "x-limit": 3.5
}
```

