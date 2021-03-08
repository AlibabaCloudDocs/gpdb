# DescribeSQLCollectorPolicy

You can call this operation to query whether the SQL collection feature is enabled for an instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeSQLCollectorPolicy&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeSQLCollectorPolicy|The operation that you want to perform. Set the value to DescribeSQLCollectorPolicy. |
|DBInstanceId|String|Yes|gp-xxxxxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|ABB39CC3-4488-4857-905D-2E4A051D0521|The ID of the request. |
|SQLCollectorStatus|String|Enable|The status of SQL collection.

 -   Enable: SQL collection is enabled.
-   Disabled: SQL collection is disabled. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeSQLCollectorPolicy
&DBInstanceId=gp-xxxxxxxxx
&<Common request parameters>
```

Sample success responses

`XML` format

```
<DescribeSQLCollectorPolicyResponse>
	  <RequestId>ABB39CC3-4488-4857-905D-2E4A051D0521</RequestId>
	  <SQLCollectorStatus>Enable</SQLCollectorStatus>
</DescribeSQLCollectorPolicyResponse>
```

`JSON` format

```
{
    "RequestId":"ABB39CC3-4488-4857-905D-2E4A051D0521",
    "SQLCollectorStatus":"Enable"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

