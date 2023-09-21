## SEMPER Policy-Elements <a id="policy_elements"></a> [🔝](#top)

The SEMPER Policies always have the following sections:

```json {linenos=table,hl_lines=[],linenostart=50}
{
  "metaData": {...},
  "configure" | "filtering" | "enrichment": {
    "policyScope": <policyScope>,
    ...
    <typeSpecificSection>
    ...
  },
  "auditing": {...}
}
```

| Key        | Value-Type | Comment |
| :---       | :---  | :---  |
| metaData   | object | (optional but recommended): provide here any attributes helping you to organize your policies. <br> *e.g. versioning, title, description, policy-type, ownership*   |
| configure *or* <br> filtering *or* <br> enrichment | object | determines the [type](#policy_types) of the policy  |
| .policyScope | object | (optional) as described in the following chapter [Section policyScope](#policy_scope). |
| .\<typeSpecificSection\> | object | will be described in the chapter [SEMPER Policy-Types](#policy_types). |
| auditing    | object | (optional but recommended) provide here any attributes helping you to audit and reasses your policies. <br> *e.g. lastAttestationDate, contact-details of auditor*  |

