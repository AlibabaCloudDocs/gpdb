# ModifySQLCollectorPolicy

You can call this operation to enable or disable the SQL collection feature for an instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ModifySQLCollectorPolicy&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifySQLCollectorPolicy|The operation that you want to perform. Set the value to ModifySQLCollectorPolicy. |
|DBInstanceId|String|Yes|gp-xxxxxxxx|The ID of the instance. |
|SQLCollectorStatus|String|Yes|Enable|Specifies whether to enable or disable SQL collection.

 -   Enable: enables SQL collection.
-   Disabled: disables SQL collection. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|4FA1F1D1-50A6-4F60-9A78-5752F2076A53|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifySQLCollectorPolicy
&DBInstanceId=gp-xxxxxxxx
&SQLCollectorStatus=Enable
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ModifySQLCollectorPolicyResponse>
           <RequestId>4FA1F1D1-50A6-4F60-9A78-5752F2076A53</RequestId>
</ModifySQLCollectorPolicyResponse>
```

`JSON` format

```
{
  "RequestId":"4FA1F1D1-50A6-4F60-9A78-5752F2076A53"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

