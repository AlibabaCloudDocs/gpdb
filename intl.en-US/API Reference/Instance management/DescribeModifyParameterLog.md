# DescribeModifyParameterLog

You can call this operation to query the parameter reconfiguration logs of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeModifyParameterLog&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeModifyParameterLog|The operation that you want to perform. Set the value to DescribeModifyParameterLog. |
|DBInstanceId|String|Yes|gp-xxxxxx|The ID of the instance. |
|StartTime|String|No|2020-02-02T11:22:22Z|The beginning of the time range to query. |
|EndTime|String|No|2020-05-05T11:22:22Z|The end of the time range to query. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Changelogs|Array of Changelogs| |Details about the parameter reconfiguration logs. |
|EffectTime|String|2020-05-05T11:22:22Z|The time when the configuration change takes effect. |
|ParameterName|String|testkey|The name of the parameter. |
|ParameterValid|String|true|Indicates whether the configuration change takes effect. |
|ParameterValueAfter|String|100|The original value of the parameter. |
|ParameterValueBefore|String|200|The new value of the parameter. |
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|The ID of the request. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeModifyParameterLog
&<Common request parameters>
&DBInstanceId=gp-xxxxxx
&StartTime=2020-02-02T11:22:22Z
&EndTime=2020-05-05T11:22:22Z
```

Sample success responses

`XML` format

```
<RequestId>F805CBCF-4BE4-410B-9FF8-41842799E5B8</RequestId>
<Changelogs>
    <ParameterValueBefore>10800000</ParameterValueBefore>
    <EffectTime>2021-03-16T11:48:14Z</EffectTime>
    <ParameterName>statement_timeout</ParameterName>
    <ParameterValueAfter>11800000</ParameterValueAfter>
    <ParameterValid>false</ParameterValid>
</Changelogs>
```

`JSON` format

```
{
  "RequestId": "F805CBCF-4BE4-410B-9FF8-41842799E5B8",
  "Changelogs": [
    {
      "ParameterValueBefore": "10800000",
      "EffectTime": "2021-03-16T11:48:14Z",
      "ParameterName": "statement_timeout",
      "ParameterValueAfter": "11800000",
      "ParameterValid": "false"
    }
  ]
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

