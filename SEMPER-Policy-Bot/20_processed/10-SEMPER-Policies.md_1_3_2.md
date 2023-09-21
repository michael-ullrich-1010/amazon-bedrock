### Section "policyScope" <a id="policy_scope"></a> [🔝](#top)

You can specify on a finegrained level in which member account and in which AWS region a SEMPER policy should be applied.

![aws-organization-account-model](docs/aws-organization-account-model.png)

An AWS account has the following account attributes <a id="account_attributes"></a>.

```json {linenos=table,hl_lines=[],linenostart=50}
<Source JSON> = {
  "accountAttibutes": {
    "accountId": 'string',
    "accountName": 'string',
    "accountTags": [
        'key1': 'value1',
        'key2': 'value2'
    ],
    "ouId": 'string',
    "ouName": 'string',
    "ouNameWithPath": 'string'
  }
}
```

So an AWS resource has the account attributes and additionaly the region it is provisioned to.

The *policyScope-Section* allows you to specify

- an **account-scope** given through *AWS account* and *Organization Unit (OU)* aspects
- a **region-scope** given through the *region code* of an AWS resource.

```json {linenos=table,hl_lines=[],linenostart=50}
{
    ...
    "policyScope": {
      "accountScope": Account-Scope Pattern JSON-Object | [Account-Scope Pattern JSON-Objects],
      "regionScope": Effective List Pattern JSON-Object
    }
    ...
}
```

| Key             | Value-Type             | Comment |
| :---            | :---                   | :---  |
| policyScope     | object                 | necessary for each new rule |
| .accountScope   | object                 | (optional) see JSON-Engine [Object in Scope](https://github.com/nuvibit/semper-policy-repo-sample/wiki/JSON_Engine#object-in-scope--) |
| .regionScope    | object                 | (optional) see JSON-Engine [Effective List](#https://github.com/nuvibit/semper-policy-repo-sample/wiki/JSON_Engine#get-effective-list--) |

