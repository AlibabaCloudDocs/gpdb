# ListTagResources

调用ListTagResources查询一个或多个AnalyticDB PostgreSQL实例已经绑定的标签列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ListTagResources&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|否|ListTagResources|系统规定参数。取值：ListTagResources |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~86912~~)查看最新的阿里云地域列表。 |
|ResourceType|String|是|instance|资源类型。取值范围：

 -   `instance`：预留模式实例。
-   `ALIYUN::GPDB::INSTANCE`：弹性模式实例。 |
|ResourceId.N|RepeatList|否|gp-xxxxxxxxxx|实例ID。N的取值范围：1~50 |
|Tag.N.Key|String|否|TestKey|标签键。标签键长度的取值范围：1~128。N的取值范围：1~20

 `Tag.N`用于精确查找绑定了指定标签的AnalyticDB PostgreSQL实例，由一个键值对组成。

 -   仅指定`Tag.N.Key`时，则返回关联该标签键的所有实例。
-   仅指定`Tag.N.Value`，则报错`InvalidParameter.TagValue`。
-   同时指定多个标签键值对时，仅同时满足所有标签键值对的AnalyticDB PostgreSQL实例会被查找到。 |
|Tag.N.Value|String|否|TestValue|标签值。标签值长度的取值范围：1~128。N的取值范围：1~20 |
|NextToken|String|否|caeba0bbb2be03f84eb48b699f0a4883|下一个查询开始Token。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|NextToken|String|caeba0bbb2be03f84eb48b699f0a4883|下一个查询开始Token。 |
|RequestId|String|5414A4E5-4C36-4461-95FC-23757A20B5F8|请求ID。 |
|TagResources|Array of TagResource| |由实例及其标签组成的集合，包含了实例ID、实例类型和标签值等信息。 |
|TagResource| | | |
|ResourceId|String|gp-xxxxxxxxxx|实例ID。 |
|ResourceType|String|instance|资源类型。 |
|TagKey|String|TestKey|标签键。 |
|TagValue|String|TestValue|标签值。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListTagResources
&RegionId=cn-hangzhou
&ResourceType=instance
&<公共请求参数>
```

正常返回示例

`XML` 格式

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

`JSON` 格式

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

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

