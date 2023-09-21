# Nuvibit SEMPER Policy Documentation <a id="top">

<!-- LOGO -->
<a href="https://nuvibit.com">
    <img src="https://nuvibit.com/images/logo/logo-nuvibit-badge.png" alt="nuvibit logo" title="nuvibit" align="right" width="100" />
</a>

<!-- SHIELDS -->
[![Maintained by nuvibit.com][nuvibit-shield]][nuvibit-url] ![Copyright by nuvibit.com][nuvibit-copyright-shield]

This is the documentation of the SEMPER Policy-Types.

Required Semper Version: >= 1.7.0

## Content <a id="top"></a>

- [SEMPER Policy-Repository](#policy_repository)
- [SEMPER Policy-Elements](#policy_elements)
  - [SEMPER Policy-Syntax](#policy_syntax)
  - [Section "policyScope"](#policy_scope)
    - [Sub-Section "accountScope"](#account_scope)
    - [Sub-Section "regionScope"](#region_scope)
    - [Samples for "policyScope"](#policy_scope_samples)
- [SEMPER Policy-Types](#policy_types)
  - [Configure-Policies](#policy_type_configure)
    - [AWS Config Rule Policies](#policy_type_configure_config)
    - [AWS EventBridge Rule Policies](#policy_type_configure_eventbridge)
    - [AWS Security Hub Policies](#policy_type_configure_securityhub)
  - [Filter-Policies](#policy_type_filter)
    - [Samples of Filter-Policies](#filter_policy_samples)
  - [Extension-Policies](#policy_type_extension)
    - [Use-Case SQS Fan-Out](#extension_sqs_fanout)
    - [Use-Case Send to AWS Security Hub](#extension_securityhub)
    - [Samples of Extension-Policies](#extension_policy_samples)
- [Processed SEMPER SecurityFinding](#processed_security_finding)

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

### SEMPER Policy Syntax <a id="policy_syntax"></a> [🔝](#top)

The SEMPER policy-syntax is described on this [wiki-page](https://github.com/nuvibit/semper-policy-repo-sample/wiki/JSON_Engine)

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

#### Sub-Section "regionScope" <a id="region_scope"></a> [🔝](#top)

In the **SEMPER Core Security** module you can specify the target regions where SEMPER will provision *AWS Config Rules*, *AWS EventBus Rules* and customize *AWS Security Hub* in the member accounts.
The section **regionScope** allows you per policy to override this settings using the [AWS region names](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html#Concepts.RegionsAndAvailabilityZones.Regions) (e.g. eu-central-1, us-west-2, more examples see below):

```json {linenos=table,hl_lines=[],linenostart=50}
{
      ...
      "regionScope": {
        "exclude": "*" | ['string'],
        "forceInclude": ['string']
      }
      ...
}
```

| Key            | Value-Type | Comment |
| :---           | :---  | :---  |
| regionScope   | object | (optional) first the optional **exclude**-section is evaluated, then the optional **forceInclude** section. |
| .exclude      | "*" or array of string | (optional) the elements in this section are evaluated using a *logical OR*. |
| .forceInclude | array of string | (optional) the elements in this section are evaluated using a *logical OR*. |

#### Samples for "policyScope" <a id="policy_scope_samples"></a> [🠕](#top)

Sample 1: Exclude all regions and include "us-east-1" only.
Via **exclude** all regions will be ignored and via **forceInclude** one specific region will be put into scope again (take care that [SCPs](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html) might prevent resources in regions):

```json {linenos=table,hl_lines=[],linenostart=50}
Sample:
{
    ...
    "policyScope": {
      "regionScope": {
        "exclude": "*",
        "forceInclude": [
          "us-east-1"
        ]
      }
    }
    ...
}
```

Sample 2: Include all accounts that have account-tag "AccountName" starting with "Core".<br>
With th **exclude** section we ignore all possible account-contexts and with the **forceInclude** we include all accounts that have a tag-value prefixed with "Core" for the tag-key "AccountName".

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "policyScope": {
    "accountScope": {
      "exclude": "*",
      "forceInclude": {
        "accountTags": {
          "AccountName" : [
            {
              "prefix": "Core"
            }
          ]
        }
      }
    }
  }
  ...
}
```

Sample 3: Include all accounts that have account-tag "Environment" != "Production"

```json {linenos=table,hl_lines=[],linenostart=50}
This
{
  ...
  "policyScope": {
    "accountScope": {
      "exclude": "*",
      "forceInclude": {
        "accountTags": {
          "Environment" : [
            {
              "anything-but": "Production"
            }
          ]
        }
      }
    }
  }
  ...
}
is the same like:
{
  ...
  "policyScope": {
    "accountScope": {
      "exclude": {
        "accountTags": {
          "Environment" : [
            "Production"
          ]
        }
      }
    }
  }
  ...
}
```

## SEMPER Policy-Types <a id="policy_types"></a> [🔝](#top)

SEMPER distinguishes between different policy types:

- [Configure-Policies](#policy_type_configure)
- [Filter-Policies](#policy_type_filter)
- [Extension-Policies](#policy_type_extension)

### Configure-Policies <a id="policy_type_configure"></a> [🔝](#top)

SEMPER will crawl through all accounts in your AWS Organization and assume the SEMPER Member role in the each account.
Each account-context (AWS account id, OU-ID, AWS account tags) will be determined.

Then SEMPER will iterate through all policies in the folders:
> Folder: /10_configure/config_rules
> Folder: /10_configure/event_rules
> Folder: /10_configure/securityhub_controls (when /10_configure/securityhub_standards.json is specified)

With the optional [policyScope](policy_scope) you can specify, if the configure policy will be applied to the member account.
The configure-action specified in the policy will only be applied if the optional policyScope evaluates to `true`.

#### AWS Config Rule Policies <a id="policy_type_configure_config"></a> [🔝](#top)

> Folder: [/10_configure/config_rules](/10_configure/config_rules)

This policies allow you to specify the provisioning of custom AWS Config Rule to your member accounts.
SEMPER uses boto3 [ConfigService.Client.put_config_rule](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/config.html#ConfigService.Client.put_config_rule) for this feature.

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "configure": {
    "policyScope": {...},
    "configRuleSettings": {
      "configRuleName": 'string',
      "configRuleDescription": 'string',
      "complianceResourceTypes": ['string'],
      "sourceOwner": "CUSTOM_LAMBDA" | "AWS",
      "sourceIdentifier": 'string',
      "sourceDetails": [
        {
          "messageType": 'string',
          "maximumExecutionFrequency": 'string'
        }
      ]
      "inputParameters ": 'string'
    }
  }
  ...
}
```

| Key                            | Value-Type   | Comment |
| :---                           | :---         | :---    |
| .policyScope                   | object       | (optional) as described in this chapter [Section policyScope](#policy_scope). |
| .configRuleSettings            | object       | specifying the attributes used for the boto3 call. |
| . .configRuleName              | string       | according to boto3 [ConfigRuleName](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/config.html#ConfigService.Client.put_config_rule)-specification - will be prefixed with ***semper-***.|
| . .configRuleDescription       | string       | (optional) according to boto3 [Description](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/config.html#ConfigService.Client.put_config_rule)-specification.      |
| . .complianceResourceTypes     | array of string |  (optional) according to boto3 [Scope.ComplianceResourceTypes](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/config.html#ConfigService.Client.put_config_rule)-specification |
| . .sourceOwner                 | string       | Either "CUSTOM_LAMBDA" for a custom Lambda function in the Core Security account or "AWS" for AWS managed Config rules. |
| . .sourceIdentifier            | string       | If "sourceOwner" is "CUSTOM_LAMBDA", name of the custom evaluation Lambda hosted in the Core Security that is valid for this Config Rule. <br> If "sourceOwner" is "AWS", identifier of the AWS managed rule. List of [AWS Config Managed Rules](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html). E.g. *IAM_PASSWORD_POLICY* |
| . .sourceDetails               | list(object) | **_NOTE:_** Required only if "sourceOwner" is "CUSTOM_LAMBDA". |
| . . .messageType               | string       | According to [AWS Config Managed Rules](https://docs.aws.amazon.com/config/latest/APIReference/API_SourceDetail.html) API Reference |
| . . .maximumExecutionFrequency | string       | (Only required if messageType is 'ScheduledNotification') According to [AWS Config Managed Rules](https://docs.aws.amazon.com/config/latest/APIReference/API_SourceDetail.html) API Reference |
| . .inputParameters             | string       | (optional) A string, in JSON format, that is passed to the Config rule Lambda function. |

#### AWS EventBridge Rule Policies <a id="policy_type_configure_eventbridge"></a> [🔝](#top)

> Folder: [/10_configure/event_rules](/10_configure/event_rules)

This policies allow you to provision custom AWS EventBridge Rules to your member accounts.
SEMPER uses boto3 [Events.Client.put_rule](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule) for this feature.

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "configure": {
    "policyScope": {...},
    "eventSettings": {
      "eventName": 'string' according to boto3 Name-specification - will be prefixed with "semper-",
      "eventDescription": 'string' according to boto3 Description-specification,
      "eventPattern": 'string' following the specification described in the link below
    }
  }
  ...
}
```

| Key                 | Value-Type | Comment |
| :---                | :---  | :---  |
| .policyScope        | object | (optional) as described in this chapter [Section policyScope](#policy_scope). |
| .eventSettings      | object | specifying the attributes used for the boto3 call. |
| . .eventName        | string | according to boto3 [Name](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule)-specification - will be prefixed with "semper-".|
| . .eventDescription | string | according to boto3 [Description](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule)-specification. |
| . .eventPattern     | array of string | The eventPattern has to follow this AWS specification: [Amazon EventBridge event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html) |

#### AWS Security Hub Configuration Policies <a id="policy_type_configure_securityhub"></a> [🔝](#top)

For AWS Security Hub please consider this diagram:

![aws-security-hub-model](docs/aws-security-hub-model.png)

For AWS Security Hub tailoring we have two types of policies - a policy for tailoring the standards and a policy-folder for tailoring the controls:
> Policy: [/10_configure/securityhub_standards.json](/10_configure/securityhub_standards.json)
> Folder: [/10_configure/securityhub_controls/](/10_configure/securityhub_controls)

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "configure": {
    "securityHubStandards": [
      {
        "standardName": "AWS Foundational Security Best Practices",
        "standardIdentifier": "aws-foundational-security-best-practices/v/1.0.0",
        "standardArn": "arn:aws:securityhub:${region}::standards/aws-foundational-security-best-practices/v/1.0.0",
        "targetState": "ENABLED",
        "policyScope": {}
      },
      ...
    ]
  }    
  ...
}
```

| Key                 | Value-Type | Comment |
| :---                | :---  | :---  |
| .policyScope        | object | (optional) as described in this chapter [Section policyScope](#policy_scope). |
| .eventSettings      | object | specifying the attributes used for the boto3 call. |
| . .eventName        | string | according to boto3 [Name](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule)-specification - will be prefixed with "semper-".|
| . .eventDescription | string | according to boto3 [Description](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule)-specification. |
| . .eventPattern     | array of string | The eventPattern has to follow this AWS specification: [Amazon EventBridge event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html) |

This policies allow you to enable AWS Security Hub  custom AWS EventBridge Rules to your member accounts.
SEMPER uses boto3 [Events.Client.put_rule](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule) for this feature.

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "configure": {
    "policyScope": {...},
    "eventSettings": {
      "eventName": 'string' according to boto3 Name-specification - will be prefixed with "semper-",
      "eventDescription": 'string' according to boto3 Description-specification,
      "eventPattern": 'string' following the specification described in the link below
    }
  }
  ...
}
```

| Key                 | Value-Type | Comment |
| :---                | :---  | :---  |
| .policyScope        | object | (optional) as described in this chapter [Section policyScope](#policy_scope). |
| .eventSettings      | object | specifying the attributes used for the boto3 call. |
| . .eventName        | string | according to boto3 [Name](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule)-specification - will be prefixed with "semper-".|
| . .eventDescription | string | according to boto3 [Description](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/events.html#EventBridge.Client.put_rule)-specification. |
| . .eventPattern     | array of string | The eventPattern has to follow this AWS specification: [Amazon EventBridge event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html) |

### Filter-Policies <a id="policy_type_filter"></a> [🔝](#top)

SEMPER will aggregate all the security events from the provisioned SEMPER AWS Config Rules, the AWS EventBridge Rules (API calls via CloudTrail) and also the AWS Security Hub- and Amazon GuardDuty findings. SEMPER forwards all those security findings to a SEMPER Processing Lambda in your Core Security account.

This SEMPER Processing Lambda will determine the account-context (OU-ID, AWS account tags) of the originating account based on the account ID.

Depending on the source (AWS Event, AWS Security Hub or Amazon GuardDuty) SEMPER will iterate through all filtering policies in the following folders:
>Folder: /20_filtering/cloudtrail_api_calls
>Folder: /20_filtering/guardduty_findings
>Folder: /20_filtering/securityhub_findings

Based on the defined filter policies in those folders the processing Lambda will filter the the security findings

The generic structure of a SEMPER Filter-Policy looks like this:

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "filtering": {
    "policyScope": {...},
    "findingPattern": {...}
  }
  ...
}
```

| Key             | Value-Type | Comment |
| :---            | :---  | :---  |
| filtering       | object | |
| .policyScope    | object | (optional) Each Security Finding has an originating account and an AWS region of the emitting resource. <br>As described in this chapter [Section policyScope](#policy_scope) you can limit the application of a SEMPER policy to a specific account- and region-context. |
| .findingPattern | object | According to the [SEMPER Policy Syntax](#policy_syntax). |

#### Samples of Filter-Policies <a id="filter_policy_samples"> [🔝](#top)

```json {linenos=table,hl_lines=[],linenostart=50}
{
  "metaData": {
    "domain": "filter",
    "type": "eventbridge_rule",
    "title": "Ignore KMS events for all environments except 'Production'",
    ...
  },
  "filtering": {
    "policyScope": {
      "accountScope": {
        "exclude": "*",
        "forceInclude": {
          "accountTags": {
            "Environment" : [
              {
                "anything-but": "Production"
              }
            ]
          }
        }
      }
    },
    "findingPattern": {
      "detail": {
        "eventSource": [
          "kms.amazonaws.com"
        ],
        "eventName": [
          "DisableKey",
          "ScheduleKeyDeletion"
        ]
      }
    }
  },
  ...
}
```

```json {linenos=table,hl_lines=[],linenostart=50}
{
  "metaData": {
    "domain": "filter",
    "type": "eventbridge_rule",
    "title": "Drop CIS_AWS_1.4 findings for IAM User foundation_checkpoint_reader",
    ...
  },
  "filtering": {
    "findingPattern": {
      "ProductFields": {
        "StandardsGuideArn": [
          {
            "prefix": "arn:aws:securityhub:::ruleset/cis-aws-foundations-benchmark"
          }
        ],
        "RuleId": [
          "1.4"
        ]
      },
      "Resources": {
        "Id": [
          {
            "suffix": ":user/checkpoint_api"
          }
        ]
      }
    }
  },
  ...
}
```

### Extension-Policies <a id="policy_type_extension"></a> [🔝](#top)

Extension policies allow you a policy based extension of the finding json for post-processing stages.

Extension policies will be processed after the Filtering-Step and are located in the followinf folder:
>Folder: /30_extension

The generic structure of a SEMPER Extension-Policy looks like this:

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "extension": {
    "policyScope": {...},
    "findingPattern": {...},
    "extensionBlock": {
      "useCaseIdentifier": [
        {

        }
      ]
    }
  }
  ...
}
```

| Key             | Value-Type | Comment |
| :---            | :---  | :---  |
| extension       | object | |
| .policyScope    | object | (optional) Each Security Finding has an originating account and an AWS region of the emitting resource. <br>As described in this chapter [Section policyScope](#policy_scope) you can limit the application of a SEMPER policy to a specific account- and region-context. |
| .findingPattern | object | According to the [SEMPER Policy Syntax](#policy_syntax). |
| .extensionBlock | object | Security Findings that match the findingPattern will be extended by the extensionBlock-JSON. |

#### Use-Case SQS Fan-Out <a id="extension_sqs_fanout"></a> [🔝](#top)

The concept of this use-case is, that SQS URLs can be specified, where the processed Security Finding will be sent to.
The SQS Consumer (Lambda) will receive the full JSON of the processed Security Finding and can take the full context to determine further steps.
Examples are:

- Auto Remediation
- Notification of Workload-Owners
- Notification of Security-Team

>JSON-Key: "sqsFanOut"

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "extension": {
    ...
    "extensionBlock": {
      "sqsFanOut": [
        {
          "sqsName": "ar-close-sg-ports-trigger"
          ...
        },
        {
          "sqsUrl": "https://sqs.eu-central-1.amazonaws.com/*accountId*/notify-technical-account-owner-trigger"
          ...
        }        
      ]
    }
  }
  ...
}
```

| Key             | Value-Type | Comment |
| :---            | :---  | :---  |
| extension       | object | |
| .extensionBlock | object | Security Findings that match the findingPattern will be extended by the extensionBlock-JSON. |
| ..sqsFanOut     | list   | The list of SQS Fan-Out specifications that will be executed in parallel. |
| ...sqsUrl       | string | (conflicts with sqsName) Security Finding will be sent to the specified SQS URL. |
| ...sqsName      | string | (conflicts with sqsURL) Security Finding will be sent to the specified SQS Name in the SEMPER region of the Core Security account. |
| ...Dynmamic Key | string | (optional) For the processor of the further context information can be provided. |

#### Use-Case Send to AWS Security Hub<a id="extension_securityhub"></a> [🔝](#top)

By specifying this extension block, you can create a Security Hub Finding based on the processed SEMPER Security Finding.

>JSON-Key: "sendToSecurityHub"

```json {linenos=table,hl_lines=[],linenostart=50}
{
  ...
  "extension": {
    ...
    "extensionBlock": {
      "sendToSecurityHub": {
        "findingTitle": "SEMPER Finding Title",
        "findingSevertiy": 20,
        "findingComplianceStatus" : "FAILED",
        "findingResource" : {
          "id": "raw.detail.requestParameters.groupId",
          "type": "AwsEc2SecurityGroup"
        }
      }        
    }
  }
  ...
}
```

| Key                   | Value-Type | Comment |
| :---                  | :---  | :---  |
| extension             | object | |
| .extensionBlock       | object | Security Findings that match the findingPattern will be extended by the extensionBlock-JSON. |
| ..sendToSecurityHub   | object | |
| ...findingTitle       | string | (optional, default = 'SEMPER Finding') |
| ...findingDescription | string | (optional) |
| ...findingSevertiy    | number | (optional, default = 20) according to [AWS documentation](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_Severity.html). |
| ...findingComplianceStatus     | string | (optional, default = FAILED) according to [AWS documentation](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_Compliance.html#securityhub-Type-Compliance-Status). |
| ...findingResource    | object | (optional) |
| ....id                | string | (optional, default = '') you can self-reference the JSON of the processed Security Finding. <br> Example: 'raw.detail.requestParameters.keyId' |
| ....type              | string | (optional, default = 'AwsAccount') according to [AWS documentation](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_ResourceDetails.html). |

##### Samples of Extension-Policies <a id="extension_policy_samples"> [🔝](#top)

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

###### Sample 2 [🔝](#top)

```json {linenos=table,hl_lines=[],linenostart=50}
{
  "metaData": {
    "domain": "extension",
    "title": "AWS Account root-user was used",
    ...
  },
  "extension": {
    "findingPattern": {
      "detail-type": [
        "AWS API Call via CloudTrail",
        "AWS Console Sign In via CloudTrail"
      ],
      "detail": {
        "userIdentity": {
          "type": "Root"
        }
      }
    },
    "extensionBlock": {
      "sqsFanOut": [
        {
          "sqsName": "instant-alarm-trigger"
        }
      ],
      "sendToSecurityHub": {
        "findingTitle": "Usage of root-user",
        "findingSevertiy": 95,
        "findingComplianceStatus" : "FAILED",
      }        
    }
  }
}
```

## Processed Security Finding <a id="processed_security_finding"></a> [🔝](#top)

A processed Security Finding will sent to one of two SNS topics.
The JSON of the processed Security Finding is shown below.

```json {linenos=table,hl_lines=[],linenostart=50}
{
    "normalized": {
        "rawHash": "981a50a50765abebb04e104ea7e6e8f4",
        "time": "2023-02-10T13:38:32Z",
        "ouId": "ou-srnr-k9szj6mg",
        "ouName": "Branding",
        "ouNameWithPath": "Root/Branding/",
        "accountId": "***",
        "accountName": "aws-c1-security",
        "accountTags": {
            "account_email": "accounts+aws-c1-security@nuvibit.com",
            "environment": "prod",
            "recycled": "false",
            "account_name": "aws-c1-security",
            "deploy_context": "true",
            "share_remote_state": "false",
            "realm": "c1",
            "title": "Security account - customer1 stage",
            "account_owner": "michael.ullrich@nuvibit.com",
            "tenant": "nuvibit",
            "tfc_execution_mode": "remote"
        },
        "region": "us-east-1",
        "sourceService": "AwsCloudTrail",
        "findingName": "iam.amazonaws.com > DeleteRolePermissionsBoundary",
        "severity": "unknown"
    },
    "extensions": {
        "sqsFanOut": [
            {
                "sqsUrl": "https://sqs.eu-central-1.amazonaws.com/***/ar-attach-permission-boundary-policy-trigger"
            }
        ]
    },
    "raw": {
        "version": "0",
        "id": "d0fbe1ff-e281-4f61-ea20-b493c9a47826",
        "detail-type": "AWS API Call via CloudTrail",
        "source": "aws.iam",
        "account": "***",
        "time": "2023-02-10T13:38:32Z",
        "region": "us-east-1",
        "resources": [],
        "detail": {
            "eventVersion": "1.08",
            "userIdentity": {
                "type": "AssumedRole",
                "principalId": "AROAZD2VZY6Q7PHCJQXNT:michael.ullrich@nuvibit.com",
                "arn": "arn:aws:sts::***:assumed-role/AWSReservedSSO_AdministratorAccess_33d40419b45cd0b4/michael.ullrich@nuvibit.com",
                "accountId": "***",
                "accessKeyId": "ASIAZD2VZY6QWQZ4R4OJ",
                "sessionContext": {
                    "sessionIssuer": {
                        "type": "Role",
                        "principalId": "AROAZD2VZY6Q7PHCJQXNT",
                        "arn": "arn:aws:iam::***:role/aws-reserved/sso.amazonaws.com/eu-central-1/AWSReservedSSO_AdministratorAccess_33d40419b45cd0b4",
                        "accountId": "***",
                        "userName": "AWSReservedSSO_AdministratorAccess_33d40419b45cd0b4"
                    },
                    "webIdFederationData": {},
                    "attributes": {
                        "creationDate": "2023-02-10T10:54:16Z",
                        "mfaAuthenticated": "false"
                    }
                }
            },
            "eventTime": "2023-02-10T13:38:29Z",
            "eventSource": "iam.amazonaws.com",
            "eventName": "DeleteRolePermissionsBoundary",
            "awsRegion": "us-east-1",
            "sourceIPAddress": "87.245.73.179",
            "userAgent": "AWS Internal",
            "requestParameters": {
                "roleName": "App_test2"
            },
            "responseElements": null,
            "requestID": "a4b9f149-7e58-4bf1-9d28-e56ddfda1392",
            "eventID": "38c92063-09e4-4397-a96a-6a506188965f",
            "readOnly": false,
            "eventType": "AwsApiCall",
            "managementEvent": true,
            "recipientAccountId": "***",
            "eventCategory": "Management",
            "sessionCredentialFromConsole": "true"
        }
    }
}
```

| Key         | Value-Type | Comment |
| :---        | :---  | :---  |
| normalized  | object | Meta-Data of the AWS CloudTrail Event like Account Context. |
| extensions  | object | If extension policies matched, they are stated at this JSON Object. |
| raw         | object | The original AWS JSON document of the AWS CloudTrail Event. |

<!-- MARKDOWN LINKS & IMAGES -->
[nuvibit-shield]: https://img.shields.io/badge/maintained%20by-nuvibit.com-%235849a6.svg?style=flat&color=1c83ba
[nuvibit-url]: https://nuvibit.com
[nuvibit-copyright-shield]: https://img.shields.io/badge/copyright%20by-nuvibit.com-%235849a6.svg?style=flat&color=1c83ba