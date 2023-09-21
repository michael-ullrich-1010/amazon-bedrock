### Configure-Policies <a id="policy_type_configure"></a> [🔝](#top)

SEMPER will crawl through all accounts in your AWS Organization and assume the SEMPER Member role in the each account.
Each account-context (AWS account id, OU-ID, AWS account tags) will be determined.

Then SEMPER will iterate through all policies in the folders:
> Folder: /10_configure/config_rules
> Folder: /10_configure/event_rules
> Folder: /10_configure/securityhub_controls (when /10_configure/securityhub_standards.json is specified)

With the optional [policyScope](policy_scope) you can specify, if the configure policy will be applied to the member account.
The configure-action specified in the policy will only be applied if the optional policyScope evaluates to `true`.

