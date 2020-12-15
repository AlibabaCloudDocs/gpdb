# ListTagResources

Queries tags that are bound to one or more AnalyticDB for PostgreSQL instances.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ListTagResources&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|No|ListTagResources|The operation that you want to perform. Set the value to ListTagResources. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region. You can call the [DescribeRegions](~~86912~~) operation to query region IDs. |
|ResourceType|String|Yes|instance|The mode of the instance. Valid values:

-   `instance`: reserved storage mode
-   `ALIYUN::GPDB::INSTANCE`: elastic storage mode |
|ResourceId.N|RepeatList|No|gp-xxxxxxxxxx|The ID of an instance. Valid values of N: 1 to 50. |
|Tag.N.Key|String|No|TestKey|The key of a tag. A tag key can contain 1 to 128 characters in length. Valid values of N: 1 to 20.

You can use key-value pair `Tag.N.Key and Tag.N.Value` to query AnalyticDB for PostgreSQL instances that have specific tags bound.

-   If you specify only `Tag.N.Key`, the instances that contain all the specified tag keys are returned.
-   If you specify only`Tag.N.Value`, error `InvalidParameter.TagValue` is returned.
-   If you specify multiple tag key-value pairs at a time, the instances that have all the specified tags are returned. |
|Tag.N.Value|String|No|TestValue|The value of a tag. A tag value can contain 1 to 128 characters in length. Valid values of N: 1 to 20. |
|NextToken|String|No|caeba0bbb2be03f84eb48b699f0a4883|The token that you specify to start the next query. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|NextToken|String|caeba0bbb2be03f84eb48b699f0a4883|The token that is returned for the next query. |
|RequestId|String|5414A4E5-4C36-4461-95FC-23757A20B5F8|The ID of the request. |
|TagResources|Array of TagResource| |A list of instances and tags, including the instance IDs, instance modes, and tag key-value pairs. |
|TagResource| | | |
|ResourceId|String|gp-xxxxxxxxxx|The ID of an instance. |
|ResourceType|String|instance|The mode of the instance. |
|TagKey|String|TestKey|The key of the tag. |
|TagValue|String|TestValue|The value of the tag. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ListTagResources
&RegionId=cn-hangzhou
&ResourceType=instance
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>5414A4E5-4C36-4461-95FC-23757A20B5F8</RequestId>
<NextToken>caeba0bbb2be03f84eb48b699f0a4883</NextToken>
<TagResources>
    <TagResource>
        <ResourceId>gp-xxxxxxxxxx</ResourceId>
        <TagKey>TestKey</TagKey>
        <ResourceType>instance</ResourceType>
        <TagValue>TestValue</TagValue>
    </TagResource>
</TagResources>
```

`JSON` format

```
{
    "RequestId": "5414A4E5-4C36-4461-95FC-23757A20B5F8",
    "NextToken": "caeba0bbb2be03f84eb48b699f0a4883",
    "TagResources": {
        "TagResource": {
            "ResourceId": "gp-xxxxxxxxxx",
            "TagKey": "TestKey",
            "ResourceType": "instance",
            "TagValue": "TestValue"
        }
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

