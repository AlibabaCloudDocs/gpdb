# UpgradeDBVersion

You can call this operation to upgrade the minor version of a specific AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=UpgradeDBVersion&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UpgradeDBVersion|The operation that you want to perform. Set the value to UpgradeDBVersion. |
|DBInstanceId|String|Yes|gp-wz9kmr708m155j\*\*\*|The ID of the instance. |
|MajorVersion|String|Yes|6.0|The major version of the instance. |
|MinorVersion|String|Yes|6.0|The minor version of the instance. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. |
|SwitchTimeMode|String|No|xxxxx|The upgrade method. |
|SwitchTime|String|No|xxxxx|The upgrade time. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|DBInstanceId|String|gp-wz9kmr708m155j\*\*\*|The ID of the instance. |
|DBInstanceName|String|gp-wz9kmr708m155j\*\*\*|The name of the instance. |
|RequestId|String|25C11EE5-B7E8-481A-A07C-BD619971A570|The ID of the request. |
|TaskId|String|101450956|The ID of the task. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=UpgradeDBVersion
&DBInstanceId=gp-wz9kmr708m155j***
&MajorVersion=6.0
&MinorVersion=6.0
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<TaskId>101450956</TaskId>
<RequestId>25C11EE5-B7E8-481A-A07C-BD619971A570</RequestId>
<DBInstanceId>gp-wz9kmr708m155j***</DBInstanceId>
<DBInstanceName>gp-wz9kmr708m155j***</DBInstanceName>
```

`JSON` format

```
{"TaskId":"101450956","RequestId":"25C11EE5-B7E8-481A-A07C-BD619971A570","DBInstanceId":"gp-wz9kmr708m155j***","DBInstanceName":"gp-wz9kmr708m155j***"}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

