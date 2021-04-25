# ModifyParameters

You can call this operation to modify parameters of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ModifyParameters&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifyParameters|The operation that you want to perform. Set the value to ModifyParameters. |
|DBInstanceId|String|Yes|gp-xxxxxxxx|The ID of the instance. |
|Parameters|String|Yes|\{"statement\_timeout":"11800010"\}|The list of the parameters. |
|ForceRestartInstance|Boolean|No|false|Specifies whether to forcibly restart the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F825857F-ECAF-447D-831A-441D9C8784DD|The ID of the request. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=ModifyParameters
&DBInstanceId=gp-xxxxxxxx
&Parameters={"statement_timeout":"11800010"}
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>F825857F-ECAF-447D-831A-441D9C8784DD</RequestId>
```

`JSON` format

```
{"RequestId":"F825857F-ECAF-447D-831A-441D9C8784DD"}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

