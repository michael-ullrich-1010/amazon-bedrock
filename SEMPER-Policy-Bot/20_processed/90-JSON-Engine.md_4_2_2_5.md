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

