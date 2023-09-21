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

