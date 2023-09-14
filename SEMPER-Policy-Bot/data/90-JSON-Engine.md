# Nuvibit JSON-Engine Documentation <a id="top">
<!-- LOGO -->
<a href="https://nuvibit.com">
    <img src="https://nuvibit.com/images/logo/logo-nuvibit-badge.png" alt="nuvibit logo" title="nuvibit" align="right" width="100" />
</a>

<!-- SHIELDS -->
[![Maintained by nuvibit.com][nuvibit-shield]][nuvibit-url] ![Copyright by nuvibit.com][nuvibit-copyright-shield]

This is the documentation of the Nuvibit JSON-Engine. 

# Content

- [Short JSON Intro](#json_intro)
- [Nuvibit JSON-Engine](#nuvibit_json_engine)
  - [Syntax Examples](#nuvibit_json_engine_examples)
    - [Source JSON](#nuvibit_json_engine_examples_source)
    - [Pattern JSON](#nuvibit_json_engine_examples_patterns)
      - [AND / OR](#nuvibit_json_engine_examples_and_or)
      - [Comparator "numeric"](#nuvibit_json_engine_examples_numeric)
      - [Comparator "exists"](#nuvibit_json_engine_examples_exists)
      - [Comparator "cidr-*"](#nuvibit_json_engine_examples_cidr)
      - [Comparator "regex-*"](#nuvibit_json_engine_examples_regex)
  - [Object In Scope](#object_in_scope)
  - [Get Effective List](#get_effective_list)

# Short JSON Intro <a id="json_intro"></a> [🔝](#top)

A JSON-Object can be seen as a key-value store, able to describe complex models.
The following diagram gives an overview of the most important JSON terms inspired by [www.json.org](https://www.json.org/json-en.html).

![JSON-Basics-Image]

# Nuvibit JSON-Engine <a id="nuvibit_json_engine"></a> [🔝](#top)

The principle of the Nuvibit JSON-Engine is based on the comparison of a **Pattern JSON** with a **Source JSON**.
![JSON-Engine-Image]

The syntax of the **Pattern JSON** is in alignment with [Amazon EventBridge > Create event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html#eb-create-pattern):

| Comparison | Example | Rule syntax | Matching source example |
| :---   | :---  | :---  | :---  |
| Equals | Name is "Alice" | "Name": "Alice" or "Name": [ "Alice" ]  | "Name": "Alice" |
| And | Location is "New York" and Day is "Monday" | "Location": "New York", <br/>"Day": "Monday" | "Location": "New York",<br/> "Day": "Monday" |
| Or | PaymentType is "Credit" | "Debit" | "PaymentType": [ "Credit", "Debit"] | "PaymentType": "Credit" |
| Empty | LastName is empty | "LastName": [""] | "LastName": "" |
| "Nesting" | Customer.Name is "Alice" | "Customer": JSON Object | "Customer": { "Name": "Alice" }  |
| Mix | Location is "New York" and Day is "Monday" or "Tuesday" | "Location": "New York", <br/> "Day": ["Monday", "Thuesday"] | "Location": "New York", "Day": "Monday" or <br/> "Location": "New York", "Day": "Tuesday" |

## Logical Expressions <a id="nuvibit_json_engine_logical_expressions"></a> [🔝](#top)

Additional **Logical Expressions** are supported. In this case, the JSON-Value is a JSON-Array of JSON-Objects:

<img src="docs/Logical-Expression.svg" alt="drawing" width="400"/>

```Note
**Note**
For the Nuvibit JSON-Engine the keys of the Pattern JSON are case-insensitive to the Source JSON. 
```

| Comparison | Comparator | Example | Logical Expression Syntax | Matching Source JSON |
| :---   | :---  |  :---  | :---  | :---  |
| Begins with | prefix | Region is in the US | "Region": [ {"prefix": "us-" } ] | "Region": "us-east-1" |
| Contains | contains | ServiceName contains 'database' | "ServiceName": [ <br/>  {"contains": "database" } <br/>] |  "serviceName": "employee-database-dev" |
| Does not contain | contains-not | ServiceName does not contain 'database' | "ServiceName": [ <br/>  {"contains-not": "database" } <br/>] |  "serviceName": "employee-microservice-dev" |
| Ends with | suffix | Service name ends with "-dev" | "serviceName": [ {"suffix": "-dev" } ] | "ServiceName": "employee-database-dev" |
| Not | anything-but | Weather is anything but "Raining" | "Weather": [<br/>  { "anything-but": "Raining" } <br/>] | "Weather": "Sunny" or "Cloudy" |
| Numeric (equals) | numeric | Price is 100 | "Price": [<br/>  { "numeric": [ "=", 100 ] } <br/>] | "Price": 100 |
| Numeric (range) | numeric | Price is more than 10, and less than or equal to 20 | "Price": [<br/>  { "numeric": [ ">", 10, "<=", 20 ] } <br/>] | "Price": 15 |
| Exists | exists | ProductName exists | "ProductName": [ { "exists": true } ] | "ProductName": "SEMPER" |
| Does not exist | exists | ProductName does not exist | "ProductName": [ { "exists": false } ] | n/a |
| CIDR Range | cidr-contains | IP or CIDR Range is within specified CIDR range |  "sourceIPAddress": [<br/>  { "cidr-contains": "10.0.0.0/24" } <br/>] |  "sourceIPAddress": "10.0.0.50" |
| CIDR Range | cidr-contains-not | IP or CIDR Range is not within specified CIDR range |  "sourceIPAddress": [<br/>  { "cidr-contains-not": "10.0.0.0/24" } <br/>] |  "sourceIPAddress": "10.10.0.50" |
| REGEX match | regex-match | ServiceName matches regex pattern "^prefix-\w+-prod$" |  "ServiceName": [<br/>  { "regex-match": "^prefix-\w+-prod$" } <br/>] |  "ServiceName": "prefix-database-prod" |
| REGEX not match | regex-not-match | ServiceName does not match regex pattern "^prefix-\w+-prod$" |  "ServiceName": [<br/>  { "regex-match": "^prefix-\w+-prod$" } <br/>] |  "ServiceName": "prefix-database-int" |

## Syntax Examples <a id="nuvibit_json_engine_examples"></a> [🔝](#top)

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

### Pattern JSON <a id="nuvibit_json_engine_examples_patterns"></a> [🔝](#top)

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

#### Comparator "numeric" <a id="nuvibit_json_engine_examples_numeric"></a> [🔝](#top)

**Example #1**

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "a-count": [{"numeric": [">", 0, "<", 100 ]}],
    "b-count": [{"numeric": [">", 0, "<=", 100]}],
    "c-count": [{"numeric": [">=", 0, "<", 100]}],
    "d-count": [{"numeric": [">=", 0, "<=", 100]}]
}
```

```text
-> Pattern JSON will match to the Source JSON as all conditions are met.
```

**Example #2**

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "requestParameters": {
        "ipPermissions": {
            "items": {
                "ipProtocol": "tcp",
                "fromPort": [{"numeric": ["<=", 80]}],
                "toPort": [{"numeric": [">=", 80]}]
            }
        }
    }
}
```

```text
-> Pattern JSON will match to the Source JSON as all conditions are met.
```

#### Comparator "exists" <a id="nuvibit_json_engine_examples_exists"></a> [🔝](#top)

This example will prevent a race-condition for events generated by the auto-remediation principal.

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "userIdentity": {
        "sessionContext": {
            "sessionIssuer": {
                "userName" : [
                    {
                        "exists": false
                    },
                    {
                        "exists": true,
                        "anything-but": "foundation-auto-remediation-role"
                    }      
                ]
            }
        }
    }
}
```

```text
-> Pattern JSON will match to the Source JSON as 
   "userIdentity"."sessionContext"."sessionIssuer"."userName" does not exist in the Source JSON.
   If the node ..."userName" would exist the Pattern JSON would match if 
   the ..."userName" would not be "foundation-auto-remediation-role". 
```

#### Comparator "cidr-*" <a id="nuvibit_json_engine_examples_cidr"></a> [🔝](#top)

For CIDR calculations we recommend: https://cidr.xyz/  

**"cidr-contains" Example #1**

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "sourceIPAddress": [{"cidr-contains": "87.200.73.0/24"}]
}
```

```text
-> Pattern JSON will match to the Source JSON as the 
   "sourceIPAddress": "87.200.73.179" lies within the CIDR 87.200.73.0/24.
```

**"cidr-contains" Example #2**

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "requestParameters": {
        "ipPermissions": {
            "items": {
                "ipProtocol": "tcp",
                "ipRanges": {
                    "items": {
                        "cidrIp": [{"cidr-contains": "0.0.0.0/8"}]
                    }
                }
            }
        }
    }
}
```

```text
-> Pattern JSON will match to the Source JSON as "cidrIp": "0.0.0.0/16" is part of CIDR 0.0.0.0/8
```

**"cidr-contains-not" Example #1**

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "sourceIPAddress": [{"cidr-contains-not": "87.245.73.0/24"}]
}
```

```text
-> Pattern JSON will match to the Source JSON as the 
   "sourceIPAddress": "87.200.73.179" lies not within the CIDR 87.245.73.0/24.
```

**"cidr-contains-not" Example #2**

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "requestParameters": {
        "ipPermissions": {
            "items": {
                "ipProtocol": "tcp",
                "ipRanges": {
                    "items": {
                        "cidrIp": [{"cidr-contains-not": "0.0.0.0/24"}]
                    }
                }
            }
        }
    }
}
```

```text
-> Pattern JSON will match to the Source JSON as "cidrIp": "0.0.0.0/16" is not part of CIDR 0.0.0.0/24
```

#### Comparator "regex-*" <a id="nuvibit_json_engine_examples_regex"></a> [🔝](#top)

For regex expressions we recommend: https://www.autoregex.xyz/

**"regex-match" Example #1**

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "eventType": [{"regex-match": "^Aws"}]
}
```

```text
-> Pattern JSON will match to the Source JSON as the 
   "eventType": "AwsApiCall" matches the pattern.
```

**"regex-match" Example #2**

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "eventType": [{"regex-match": "^Azure"}]
}
```

```text
-> Pattern JSON will not match to the Source JSON as the 
   "eventType": "AwsApiCall" does not match the pattern.
```

**"regex-not-match" Example #1**

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "eventType": [{"regex-not-match": "^Aws"}]
}
```

```text
-> Pattern JSON will not match to the Source JSON as the 
   "eventType": "AwsApiCall" matches the pattern.
```

**"regex-not-match" Example #2**

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "eventType": [{"regex-not-match": "^Azure"}]
}
```

```text
-> Pattern JSON will match to the Source JSON as the 
   "eventType": "AwsApiCall" does not match the pattern.
```

## Object In Scope <a id="object_in_scope"></a> [🔝](#top)

Often you have a bundle of JSON-Objects you want to treat similarly.
An example would be all AWS Accounts in an AWS Organization. The context of an AWS Account would consist of an AccountID, -name and -tags and the AWS Organization Unit (OU) information, outlined in the following JSON-Object:

```json {linenos=table,hl_lines=[],linenostart=50}
"accountContext": {
      "accountId": "123456789012",
      "accountName": "aws-core-provisioning",
      "accountTags": {
        "account_email": "accounts+aws-c1-provisioning@nuvibit.com",
        "account_name": "aws-c1-provisioning",
        "account_owner": "semper_rocks@nuvibit.com",
        "environment": "nonprod",
        "tenant": "nuvibit",
        "tfc_execution_mode": "remote",
        "title": "provisioning account - customer1 stage"
      },
      "ouId": "ou-1234-12345678",
      "ouName": "Branding",
      "ouNameWithPath": "/Branding"
    }
```

In our example, all AWS Accounts of an AWS Organization would be in scope by default.
Now let's assume, you want to select all non-prod accounts from specific Organization Units (e.g. /DMZ).

For this use case, we have introduced a **Scope Pattern JSON**.
![JSON-Engine-Scope-Pattern-Image]

It first allows you to specify an optional exclude block. The JSON-Value can either be **"*"**, which excludes all default objects or a **Pattern JSON-Object** or a list of **Pattern JSON-Objects** specifying a bundle of JSON-Objects to exclude.

```text
<Scope Pattern JSON-Key>: Scope Pattern JSON-Object
or
<Scope Pattern JSON-Key>: [Scope Pattern JSON-Object, Scope Pattern JSON-Object]

with <Scope Pattern JSON-Object> = {
    "exclude": "*" | Pattern JSON-Object | [
        Pattern JSON-Object
    ],
    "forceInclude": Pattern JSON Object | [
        Pattern JSON Object
    ]
}
```

| Key           | Value-Type                                                 | Comment    |
| :---          | :---                                                       | :---       |
| .exclude      | "*" or Pattern JSON-Object or List of Pattern JSON-Objects | (optional) |
| .forceInclude | Pattern JSON-Object or List of Pattern JSON-Objects        | (optional) |

**Object in Scope Example #1**

```json {linenos=table,hl_lines=[],linenostart=50}
"accountScope": {
    "exclude": "*",
    "forceInclude": {
        "accountName": [
            {
                "contains": "-core-"
            }
        ]
    }
}
```

```text
-> Scope Pattern JSON will select all AWS Accounts where "accountContext"."accountName" contains "-core-"
```

**Object in Scope Example #2**

```json {linenos=table,hl_lines=[],linenostart=50}
"accountScope": {
    "exclude": "*",
    "forceInclude": [
        {
            "accountTags": {
                "environment": "nonprod"
            },
            "ouNameWithPath": [
                {
                    "contains": "department_a_"
                }
            ]
        }
    ]
}
```

```text
-> Scope Pattern JSON will select all AWS Accounts where 
   "accountContext"."accountTags"."environment" equals "nonprod" and   
   "accountContext"."ouNameWithPath" contains "department_a_"
```

**Object in Scope Example #3**

```json {linenos=table,hl_lines=[],linenostart=50}
"accountScope": {
    "exclude": "*",
    "forceInclude": [
        {
            "accountTags": {
                "environment": "nonprod"
            }
        },
        {
            "ouNameWithPath": [
                {
                    "contains": "sandbox"
                }
            ]
        }
    ]
}
```

```text
-> Scope Pattern JSON will select all AWS Accounts where 
   "accountContext"."accountTags"."environment" equals "nonprod" or 
   "accountContext"."ouNameWithPath" contains "sandbox"
```

**Object in Scope Example #4**

```json {linenos=table,hl_lines=[],linenostart=50}
"accountScope": {
    "exclude": {
        "accountTags": {
            "environment": "core"
        }
    }
}
```

```text
-> Scope Pattern JSON will select all AWS Accounts that are not 
   "accountContext"."accountTags"."environment" equals "core"
```

**Object in Scope Example #5**

```json {linenos=table,hl_lines=[],linenostart=50}
"accountScope": [
    {
        "exclude": "*",
        "forceInclude": {
            "ouNameWithPath": [
                {
                    "contains": "dept_2"
                }
            ]
        }
    },
    {
        "exclude": "*",
        "forceInclude": {
            "accountTags": {
                "environment": "prod"
            },
            "ouNameWithPath": [
                {
                    "contains": "dept_1"
                }
            ]
        }
    }
]
```

```text
-> Scope Pattern JSON will select all AWS Accounts where
   "accountContext"."ouNameWithPath" contains "dept_2" or where 
   "accountContext"."accountTags"."environment" equals "prod" and 
   "accountContext"."ouNameWithPath" contains "dept_1"
```


## Get Effective List <a id="get_effective_list"></a> [🔝](#top)

The Object-In-Scope convention can also be applied to a list of strings:

```json {linenos=table,hl_lines=[],linenostart=50}
"defaultAwsRegions": [
    "eu-central-1",
    "eu-central-2",
    "us-east-1"
]
```

It first allows you to specify an optional exclude block. The JSON-Value can either be **"*"**, which excludes all default objects or a **Pattern JSON-Object** or a list of **Pattern JSON-Objects** specifying a bundle of JSON-Objects to exclude.

```text
<Effective List JSON-Key>: Effective List Pattern JSON-Object

with <Effective List JSON-Key> = {
    "exclude": "*" | "string" | [ "string" ],
    "forceInclude": "string" | [ "string" ]
}
```

| Key           | Value-Type                       | Comment    |
| :--           | :---                             | :---       |
| .exclude      | "*" or string or List of strings | (optional) |
| .forceInclude | string or List of strings        | (optional) |

**Get Effective List Example #1**

```json {linenos=table,hl_lines=[],linenostart=50}
"awsRegionsInScope": {
    "exclude": "*",
    "forceInclude": "us-west-1"
}
```

```text
-> awsRegionsEffective = [
    "us-west-1"
]
```

**Get Effective List Example #2**

```json {linenos=table,hl_lines=[],linenostart=50}
"awsRegionsInScope": {
    "exclude": "eu-central-2",
    "forceInclude": [
        "eu-north-1",
        "us-west-1"
    ]
}
```

```text
-> awsRegionsEffective = [
    "eu-central-1",
    "eu-north-1",
    "us-east-1",
    "us-west-1"
]
```

**Get Effective List Example #2**

```json {linenos=table,hl_lines=[],linenostart=50}
"awsRegionsInScope": {
    "forceInclude": [
        "eu-north-1"
    ]
}
```

```text
-> awsRegionsEffective = [
    "eu-central-1",
    "eu-central-2",
    "eu-north-1",
    "us-east-1"
]
```

<!-- MARKDOWN LINKS & IMAGES -->
[nuvibit-shield]: https://img.shields.io/badge/maintained%20by-nuvibit.com-%235849a6.svg?style=flat&color=1c83ba
[nuvibit-url]: https://nuvibit.com
[nuvibit-copyright-shield]: https://img.shields.io/badge/copyright%20by-nuvibit.com-%235849a6.svg?style=flat&color=1c83ba

[JSON-Basics-Image]: https://raw.githubusercontent.com/wiki/nuvibit/semper-policy-repo-sample/docs/JSON-Basics.svg
[JSON-Engine-Image]: https://raw.githubusercontent.com/wiki/nuvibit/semper-policy-repo-sample/docs/JSON-Engine.svg
[JSON-Engine-Scope-Pattern-Image]: https://raw.githubusercontent.com/wiki/nuvibit/semper-policy-repo-sample/docs/JSON-Engine-Scope-Pattern.svg
