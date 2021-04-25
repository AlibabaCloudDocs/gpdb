# DescribeTags

You can call this operation to query the tags of AnalyticDB for PostgreSQL instances.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeTags&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|No|DescribeTags|The operation that you want to perform. Set the value to DescribeTags. |
|RegionId|String|Yes|ap-southeast-1|The ID of the region. |
|ResourceType|String|Yes|instance|The type of the resource. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|A29EC547-B392-4340-AA4F-7C0A7B626E74|The ID of the request. |
|Tags|Array of Tag| |Details about the tags. |
|TagKey|String|user|The tag key. |
|TagValue|String|test|The tag value. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeTags
&RegionId=ap-southeast-1
&ResourceType=instance
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>A29EC547-B392-4340-AA4F-7C0A7B626E74</RequestId>
<Tags>
    <TagKey>user</TagKey>
    <TagValue>test</TagValue>
</Tags>
```

`JSON` format

```
{"RequestId":"A29EC547-B392-4340-AA4F-7C0A7B626E74","Tags":[{"TagKey":"user","TagValue":"test"}]}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

