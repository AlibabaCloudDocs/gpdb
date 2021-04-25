# AddBuDBInstanceRelation

You can call this operation to associate the BusinessUnit parameter with an instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=AddBuDBInstanceRelation&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|AddBuDBInstanceRelation|The operation that you want to perform. Set the value to AddBuDBInstanceRelation. |
|BusinessUnit|String|Yes|BusinessUnit|The description of the business unit. |
|DBInstanceId|String|Yes|gp-xxxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|BusinessUnit|String|BusinessUnit|The description of the business unit. |
|DBInstanceName|String|gp-xxxxxx|The ID of the instance. |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|The ID of the request. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=AddBuDBInstanceRelation
&DBInstanceId=gp-xxxxxx
&BusinessUnit=BusinessUnit
```

Sample success responses

`XML` format

```
<RequestId>D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD</RequestId>
<BusinessUnit>BusinessUnit</BusinessUnit>
<DBInstanceName>gp-xxxxxx</DBInstanceName>
```

`JSON` format

```
{
  "RequestId":"D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD",
  "BusinessUnit": "BusinessUnit",
  "DBInstanceName": "gp-xxxxxx"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

