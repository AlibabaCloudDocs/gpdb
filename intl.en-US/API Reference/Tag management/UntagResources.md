# UntagResources

Unbinds tags from specific AnalyticDB for PostgreSQL instances. After you unbind tags, the tags are automatically deleted if they are not bound to other instances.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=UntagResources&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|No|UntagResources|The operation that you want to perform. Set the value to UntagResources. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region. You can call the [DescribeRegions](~~86912~~) operation to query region IDs. |
|ResourceId.N|RepeatList|Yes|gp-xxxxxxxxxxx|The ID of an instance. Valid values of N: 1 to 50. |
|ResourceType|String|Yes|instance|The mode of the instance. Valid values:

 -   `instance`: reserved storage mode
-   `ALIYUN::GPDB::INSTANCE`: elastic storage mode |
|All|Boolean|No|false|Specifies whether to unbind all tags from an instance. This parameter is valid only when the TagKey.N parameter is not specified. Valid values:

 -   true
-   false

 Default value: false |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5414A4E5-4C36-4461-95FC-23757A20B5F8|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=UntagResources
&RegionId=cn-hangzhou
&ResourceId.1=gp-xxxxxxxxxxx
&ResourceType=instance
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>5414A4E5-4C36-4461-95FC-23757A20B5F8</RequestId>
```

`JSON` format

```
{
    "RequestId": "5414A4E5-4C36-4461-95FC-23757A20B5F8"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

