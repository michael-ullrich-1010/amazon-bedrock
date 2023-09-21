## SEMPER Policy-Repository <a id="policy_repository"></a> [🔝](#top)

The SEMPER Policies are stored in the Policy-Repository located in the Core Security account. SEMPER distinguishes between different policy types that will be described in later chapters:

- [Configure-Policies](#policy_type_configure)
- [Filter-Policies](#policy_type_filter)
- [Extension-Policies](#policy_type_extension)

The following folder structure is required for SEMPER and may not be altered.

There is one JSON file per policy.

In the folders marked with "..." you may place your own policies as JSON files in the respective folder. Please ensure to use the right policy type for the right folder.
If you want to disable policies, just create another subfolder (e.g. /disabled) and move the (given) policies you want to disable there.

```text
policy_repository/
│   README.md
│
├───10_configure/
│   │   securityhub.json
│   ├───config_rules/
│   │   │   semper_configure_policy.json
│   │   │   ...
│   │   │
│   │   ├───disabled/
│   │   │   semper_disabled_policy.json
│   │   │   ...
│   │
│   └───event_rules/
│   │   │   semper_configure_policy.json
│   │   │   ...
│   │
│   securityhub_controls/
│   │   │   semper_configure_policy.json
│   │   │   ...
│   │
│   securityhub_standards.json
│
├───20_filtering/
│   ├───cloudtrail_api_calls/
│   │   │   semper_filter_policy.json
│   │   │   ...
│   │
│   ├───guardduty_findings/
│   │   │   semper_filter_policy.json
│   │   │   ...
│   │
│   ├───securityhub_findings/
│   │   │   semper_filter_policy.json
│   │   │   ...
│
└───30_extension/
    │   semper_extension_policy.json
    │   ...
```

