# ModifyDBInstanceConnectionString

Modifies the internal or public endpoint and port number of an instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceConnectionString&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifyDBInstanceConnectionString|The operation that you want to perform. Set the value to ModifyDBInstanceConnectionString. |
|ConnectionStringPrefix|String|Yes|gp-xxxxxxxx|The new endpoint of the instance. |
|CurrentConnectionString|String|Yes|gp-xxxxxxx.gpdb.rds.aliyuncs.com|The original endpoint of the instance. |
|DBInstanceId|String|Yes|gp-xxxxxxx|The ID of the instance. |
|Port|String|Yes|3432|The new port number of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|29B0BF34-D069-4495-92C7-FA6D94528A9E|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceConnectionString
&ConnectionStringPrefix=gp-xxxxxxxx
&CurrentConnectionString=gp-xxxxxxx.gpdb.rds.aliyuncs.com
&DBInstanceId=gp-xxxxxxx
&Port=3432
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ModifyDBInstanceConnectionStringResponse>
           <RequestId>29B0BF34-D069-4495-92C7-FA6D94528A9E</RequestId>
</ModifyDBInstanceConnectionStringResponse>
```

`JSON` format

```
{
	"RequestId": "29B0BF34-D069-4495-92C7-FA6D94528A9E"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

