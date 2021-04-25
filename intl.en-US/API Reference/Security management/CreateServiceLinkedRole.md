# CreateServiceLinkedRole

You can call this operation to create a service-linked role \(SLR\).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=CreateServiceLinkedRole&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateServiceLinkedRole|The operation that you want to perform. Set the value to CreateServiceLinkedRole. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|The ID of the request. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=CreateServiceLinkedRole
&<Common request parameters>
&RegionId=cn-hangzhou
```

Sample success responses

`XML` format

```
<RequestId>B4CAF581-2AC7-41AD-8940-D56DF7AADF5B</RequestId>
```

`JSON` format

```
{
    "RequestId": "B4CAF581-2AC7-41AD-8940-D56DF7AADF5B"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

