# CheckServiceLinkedRole

You can call this operation to query whether a service-linked role \(SLR\) is created.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=CheckServiceLinkedRole&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CheckServiceLinkedRole|The operation that you want to perform. Set the value to CheckServiceLinkedRole. |
|RegionId|String|No|cn-hangzhou|The ID of the region. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|HasServiceLinkedRole|String|true|Indicates whether an SLR was created. |
|RegionId|String|cn-hangzhou|The ID of the region. |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|The ID of the request. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=CheckServiceLinkedRole
&<Common request parameters>
&RegionId=cn-hangzhou
```

Sample success responses

`XML` format

```
<RequestId>54B01BA7-155E-4212-B16A-E761DBD1FA97</RequestId>
<HasServiceLinkedRole>true</HasServiceLinkedRole>
<RegionId>cn-hangzhou</RegionId>
```

`JSON` format

```
{
  "RequestId": "54B01BA7-155E-4212-B16A-E761DBD1FA97",
  "HasServiceLinkedRole": true,
  "RegionId": "cn-hangzhou"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

