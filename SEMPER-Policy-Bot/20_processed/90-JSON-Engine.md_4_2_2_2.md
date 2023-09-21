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

