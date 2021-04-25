# DescribeUserEncryptionKeyList

You can call this operation to query the created Key Management Service \(KMS\) key list.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeUserEncryptionKeyList&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeUserEncryptionKeyList|The operation that you want to perform. Set the value to DescribeUserEncryptionKeyList. |
|RegionId|String|Yes|ap-southeast-1|The ID of the region. |
|PageNumber|String|No|1|The number of the page to return. Default value: 1. |
|PageSize|String|No|10|The number of KMS keys to return on each page. Default value: 10. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|KmsKeys|Array of KmsKeys| |Details about the KMS keys. |
|KeyId|String|0b8b1825-fd99-418f-875e-e4dec1dd8715|The ID of the KMS key. |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|The ID of the request. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeUserEncryptionKeyList
&RegionId=ap-southeast-1
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>B4CAF581-2AC7-41AD-8940-D56DF7AADF5B</RequestId>
<KmsKeys>
    <KeyId>0b8b1825-fd99-418f-875e-e4dec1dd8715</KeyId>
</KmsKeys>
```

`JSON` format

```
{"RequestId":"B4CAF581-2AC7-41AD-8940-D56DF7AADF5B","KmsKeys":[{"KeyId":"0b8b1825-fd99-418f-875e-e4dec1dd8715"}]}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

