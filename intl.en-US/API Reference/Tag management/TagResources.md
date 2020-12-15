# TagResources

Creates tags and binds the tags to specific AnalyticDB for PostgreSQL instances.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=TagResources&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|No|TagResources|The operation that you want to perform. Set the value to TagResources. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region. You can call the [DescribeRegions](~~86912~~) operation to query region IDs. |
|ResourceId.N|RepeatList|Yes|gp-xxxxxxxxxx|The ID of an instance. Valid values of N: 1 to 50. |
|ResourceType|String|Yes|instance|The mode of the instance. Valid values:

 -   `instance`: reserved storage mode
-   `ALIYUN::GPDB::INSTANCE`: elastic storage mode |
|Tag.N.Key|String|No|TestKey|The key of a tag. Valid values of N: 1 to 20. This parameter value cannot be an empty string. A tag key can contain a maximum of 128 characters. It cannot start with `aliyun` or`acs:` and cannot contain `http://` or`https://`. |
|Tag.N.Value|String|No|TestValue|The value of a tag. Valid values of N: 1 to 20. This parameter value can be an empty string. A tag value can contain a maximum of 128 characters. It cannot start with `acs:` and cannot contain `http://` or `https://`. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5414A4E5-4C36-4461-95FC-23757A20B5F8|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=TagResources
&RegionId=cn-hangzhou
&ResourceId.1=gp-xxxxxxxxxx
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

