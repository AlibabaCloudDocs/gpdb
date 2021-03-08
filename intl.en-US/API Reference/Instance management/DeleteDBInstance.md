# DeleteDBInstance

You can call this operation to release a pay-as-you-go instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DeleteDBInstance&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DeleteDBInstance|The operation that you want to perform. Set the value to DeleteDBInstance. |
|DBInstanceId|String|Yes|gp-xxxxxx|The ID of the instance. |
|ClientToken|String|No|0c593ea1-3bea-11e9-b96b-88e9fe637760|The client token that is used to ensure the idempotence of the request. You can use the client to generate the value, but you must make sure that it is unique among different requests. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|65BDA532-28AF-4122-AA39-B382721EEE64|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DeleteDBInstance
&DBInstanceId=gp-xxxxxx
&<Common request parameters>
```

Sample success responses

`XML` format

```
<DeleteDBInstanceResponse>  
       <RequestId>65BDA532-28AF-4122-AA39-B382721EEE64</RequestId>
</DeleteDBInstanceResponse>
```

`JSON` format

```
{
    "RequestId": "65BDA532-28AF-4122-AA39-B382721EEE64"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

