## Processed Security Finding <a id="processed_security_finding"></a> [🔝](#top)

A processed Security Finding will sent to one of two SNS topics.
The JSON of the processed Security Finding is shown below.

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "normalized": {
        "rawHash": "981a50a50765abebb04e104ea7e6e8f4",
        "time": "2023-02-10T13:38:32Z",
        "ouId": "ou-srnr-k9szj6mg",
        "ouName": "Branding",
        "ouNameWithPath": "Root/Branding/",
        "accountId": "***",
        "accountName": "aws-c1-security",
        "accountTags": {
            "account_email": "accounts+aws-c1-security@nuvibit.com",
            "environment": "prod",
            "recycled": "false",
            "account_name": "aws-c1-security",
            "deploy_context": "true",
            "share_remote_state": "false",
            "realm": "c1",
            "title": "Security account - customer1 stage",
            "account_owner": "michael.ullrich@nuvibit.com",
            "tenant": "nuvibit",
            "tfc_execution_mode": "remote"
        },
        "region": "us-east-1",
        "sourceService": "AwsCloudTrail",
        "findingName": "iam.amazonaws.com > DeleteRolePermissionsBoundary",
        "severity": "unknown"
    },
    "extensions": {
        "sqsFanOut": [
            {
                "sqsUrl": "https://sqs.eu-central-1.amazonaws.com/***/ar-attach-permission-boundary-policy-trigger"
            }
        ]
    },
    "raw": {
        "version": "0",
        "id": "d0fbe1ff-e281-4f61-ea20-b493c9a47826",
        "detail-type": "AWS API Call via CloudTrail",
        "source": "aws.iam",
        "account": "***",
        "time": "2023-02-10T13:38:32Z",
        "region": "us-east-1",
        "resources": [],
        "detail": {
            "eventVersion": "1.08",
            "userIdentity": {
                "type": "AssumedRole",
                "principalId": "AROAZD2VZY6Q7PHCJQXNT:michael.ullrich@nuvibit.com",
                "arn": "arn:aws:sts::***:assumed-role/AWSReservedSSO_AdministratorAccess_33d40419b45cd0b4/michael.ullrich@nuvibit.com",
                "accountId": "***",
                "accessKeyId": "ASIAZD2VZY6QWQZ4R4OJ",
                "sessionContext": {
                    "sessionIssuer": {
                        "type": "Role",
                        "principalId": "AROAZD2VZY6Q7PHCJQXNT",
                        "arn": "arn:aws:iam::***:role/aws-reserved/sso.amazonaws.com/eu-central-1/AWSReservedSSO_AdministratorAccess_33d40419b45cd0b4",
                        "accountId": "***",
                        "userName": "AWSReservedSSO_AdministratorAccess_33d40419b45cd0b4"
                    },
                    "webIdFederationData": {},
                    "attributes": {
                        "creationDate": "2023-02-10T10:54:16Z",
                        "mfaAuthenticated": "false"
                    }
                }
            },
            "eventTime": "2023-02-10T13:38:29Z",
            "eventSource": "iam.amazonaws.com",
            "eventName": "DeleteRolePermissionsBoundary",
            "awsRegion": "us-east-1",
            "sourceIPAddress": "87.245.73.179",
            "userAgent": "AWS Internal",
            "requestParameters": {
                "roleName": "App_test2"
            },
            "responseElements": null,
            "requestID": "a4b9f149-7e58-4bf1-9d28-e56ddfda1392",
            "eventID": "38c92063-09e4-4397-a96a-6a506188965f",
            "readOnly": false,
            "eventType": "AwsApiCall",
            "managementEvent": true,
            "recipientAccountId": "***",
            "eventCategory": "Management",
            "sessionCredentialFromConsole": "true"
        }
    }
}
```

| Key         | Value-Type | Comment |
| :---        | :---  | :---  |
| normalized  | object | Meta-Data of the AWS CloudTrail Event like Account Context. |
| extensions  | object | If extension policies matched, they are stated at this JSON Object. |
| raw         | object | The original AWS JSON document of the AWS CloudTrail Event. |

<!-- MARKDOWN LINKS & IMAGES -->
[nuvibit-shield]: https://img.shields.io/badge/maintained%20by-nuvibit.com-%235849a6.svg?style=flat&color=1c83ba
[nuvibit-url]: https://nuvibit.com
[nuvibit-copyright-shield]: https://img.shields.io/badge/copyright%20by-nuvibit.com-%235849a6.svg?style=flat&color=1c83ba
