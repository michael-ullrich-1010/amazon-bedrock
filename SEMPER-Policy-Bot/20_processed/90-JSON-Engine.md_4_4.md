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

