## Logical Expressions <a id="nuvibit_json_engine_logical_expressions"></a> [🔝](#top)

Additional **Logical Expressions** are supported. In this case, the JSON-Value is a JSON-Array of JSON-Objects:

<img src="docs/Logical-Expression.svg" alt="drawing" width="400"/>

```Note
**Note**
For the Nuvibit JSON-Engine the keys of the Pattern JSON are case-insensitive to the Source JSON. 
```

| Comparison | Comparator | Example | Logical Expression Syntax | Matching Source JSON |
| :---   | :---  |  :---  | :---  | :---  |
| Begins with | prefix | Region is in the US | "Region": [ {"prefix": "us-" } ] | "Region": "us-east-1" |
| Contains | contains | ServiceName contains 'database' | "ServiceName": [ <br/>  {"contains": "database" } <br/>] |  "serviceName": "employee-database-dev" |
| Does not contain | contains-not | ServiceName does not contain 'database' | "ServiceName": [ <br/>  {"contains-not": "database" } <br/>] |  "serviceName": "employee-microservice-dev" |
| Ends with | suffix | Service name ends with "-dev" | "serviceName": [ {"suffix": "-dev" } ] | "ServiceName": "employee-database-dev" |
| Not | anything-but | Weather is anything but "Raining" | "Weather": [<br/>  { "anything-but": "Raining" } <br/>] | "Weather": "Sunny" or "Cloudy" |
| Numeric (equals) | numeric | Price is 100 | "Price": [<br/>  { "numeric": [ "=", 100 ] } <br/>] | "Price": 100 |
| Numeric (range) | numeric | Price is more than 10, and less than or equal to 20 | "Price": [<br/>  { "numeric": [ ">", 10, "<=", 20 ] } <br/>] | "Price": 15 |
| Exists | exists | ProductName exists | "ProductName": [ { "exists": true } ] | "ProductName": "SEMPER" |
| Does not exist | exists | ProductName does not exist | "ProductName": [ { "exists": false } ] | n/a |
| CIDR Range | cidr-contains | IP or CIDR Range is within specified CIDR range |  "sourceIPAddress": [<br/>  { "cidr-contains": "10.0.0.0/24" } <br/>] |  "sourceIPAddress": "10.0.0.50" |
| CIDR Range | cidr-contains-not | IP or CIDR Range is not within specified CIDR range |  "sourceIPAddress": [<br/>  { "cidr-contains-not": "10.0.0.0/24" } <br/>] |  "sourceIPAddress": "10.10.0.50" |
| REGEX match | regex-match | ServiceName matches regex pattern "^prefix-\w+-prod$" |  "ServiceName": [<br/>  { "regex-match": "^prefix-\w+-prod$" } <br/>] |  "ServiceName": "prefix-database-prod" |
| REGEX not match | regex-not-match | ServiceName does not match regex pattern "^prefix-\w+-prod$" |  "ServiceName": [<br/>  { "regex-match": "^prefix-\w+-prod$" } <br/>] |  "ServiceName": "prefix-database-int" |

