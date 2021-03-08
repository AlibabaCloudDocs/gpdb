# ModifyDBInstanceDescription

You can call this operation to modify the description of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceDescription&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|DBInstanceDescription|String|Yes|gp-xxxxxxxx|The description of the instance. |
|DBInstanceId|String|Yes|gp-xxxxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|107BE202-D1A2-479E-98E0-A892B472562F|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceDescription
&DBInstanceDescription=gp-xxxxxxxx
&DBInstanceId=gp-xxxxxxxx
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ModifyDBInstanceDescriptionResponse>
           <RequestId>107BE202-D1A2-479E-98E0-A892B472562F</RequestId>
</ModifyDBInstanceDescriptionResponse>
```

`JSON` format

```
{
        "RequestId": "107BE202-D1A2-479E-98E0-A892B472562F"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

