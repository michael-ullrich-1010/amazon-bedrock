# Intro <a id="intro"></a> [🔝](#top)

SEMPER allows the management of AWS Security Findings via [JSON](https://en.wikipedia.org/wiki/JSON) policies.

![SEMPER-Flow-Image]

SEMPER is perfectly made for AWS Organizations (multi-account and multi-region scenarios).
The SEMPER flow is composed of the three stages **Configure**, **Processing** and **Post-Processing**.
The whole flow is managed via SEMPER Policies. This JSON poisies are stored in the SEMPER Policy-Repository located in the Core Security account.
The structure of the SEMPER Policy-Repository and the different SEMPER Policy-Types will be described in the following chapter [SEMPER Policies](./10-SEMPER-Policies).


**Configure**
Via a git-flow, SEMPER allows policy-driven selective provisioning of sensors (AWS Config Rules, AWS Event Rules) and tailoring of AWS Security Hub Standards and -Controls for your AWS accounts.

**Processing**
The following security findings get aggregted into a central AWS account and enriched with account context:

- provisioned AWS Config Rules, AWS Event Rules (Sensors form SEMPER Configure)
- Core AWS Security Hub Findings
- Core Amazon GuardDuty Findings

Based on SEMPER policies, this enriched findings can either be filtered (dropped) or enhanced with further instructions for Post-Processing like Auto-Remediation or Monitoring.

**Post-Processing**
The enriched security findings will either be sent to a "dropped"-SNS or a "relevant"-SNS topic.
From there post-processing steps can follow, like:  

- Storing to S3, CloudWatch
- Forwarding to SIEM solution
- Alarming
- Auto-Remediation



<!-- MARKDOWN LINKS & IMAGES -->
[nuvibit-shield]: https://img.shields.io/badge/maintained%20by-nuvibit.com-%235849a6.svg?style=flat&color=1c83ba
[nuvibit-url]: https://nuvibit.com
[nuvibit-copyright-shield]: https://img.shields.io/badge/copyright%20by-nuvibit.com-%235849a6.svg?style=flat&color=1c83ba

[SEMPER-Flow-Image]: https://raw.githubusercontent.com/wiki/nuvibit/semper-policy-repo-sample/docs/SEMPER-Flow.svg
