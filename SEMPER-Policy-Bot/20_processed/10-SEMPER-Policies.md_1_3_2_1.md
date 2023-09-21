#### Sub-Section "accountScope" <a id="account_scope"></a> [🔝](#top)

If a member account should be in scope, can be defined based on the [account attributes](#account_attributes) like:

- AWS account ID
- AWS account-tags (managed via the Organization Management Account)
- OU-ID

The section *accountScope* allows you to **exclude** accounts and in a second step to **forceInclude** accounts based on specific account-context information (similar approach as the tool *rsync* uses to exclude and include files and directories):

```text
"accountScope"": Scope Pattern JSON-Object
or
"accountScope": [Scope Pattern JSON-Object, Scope Pattern JSON-Object]

with <Scope Pattern JSON-Object> = {
    "exclude": "*" | Pattern JSON-Object | [Pattern JSON-Objects],
    "forceInclude": Pattern JSON Object | [Pattern JSON Objects]
}
```

| Key           | Value-Type                                                 | Comment    |
| :---          | :---                                                       | :---       |
| .exclude      | "*" or Pattern JSON-Object or List of Pattern JSON-Objects | (optional) |
| .forceInclude | Pattern JSON-Object or List of Pattern JSON-Objects        | (optional) |

