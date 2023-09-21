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

