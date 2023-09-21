###### Sample 1 [🔝](#top)

```json {linenos=table,hl_lines=[],linenostart=50}
{
  "metaData": {
    "domain": "extension",
    "title": "Auto-Remediation of TCP-Ports 22 & 3389 for CIDR range /24",
    ...
  },
  "extension": {
    "policyScope": {
      ...
    },
    "findingPattern": {
      ...
    },
    "extensionBlock": {
      "sqsFanOut": [
        {
          "sqsName": "ar-detect-sg-ports-trigger"
        }
      ]
    }
  }
}
```

