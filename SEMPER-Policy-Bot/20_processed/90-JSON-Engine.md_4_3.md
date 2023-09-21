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


