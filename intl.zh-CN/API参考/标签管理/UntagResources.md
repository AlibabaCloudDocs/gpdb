# UntagResources

调用UntagResources为指定的AnalyticDB PostgreSQL实例列表统一解绑标签。解绑后，如果该标签没有绑定其他任何实例，会被自动删除。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=UntagResources&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|否|UntagResources|系统规定参数。取值：UntagResources |
|RegionId|String|是|cn-hangzhou|地域ID，您可通过接口[DescribeRegions](~~86912~~)查看可用的地域ID。 |
|ResourceId.N|RepeatList|是|gp-xxxxxxxxxxx|实例ID。N的取值范围：1~50 |
|ResourceType|String|是|instance|资源类型。取值范围：

 -   `instance`：预留模式实例。
-   `ALIYUN::GPDB::INSTANCE`：弹性模式实例。 |
|TagKey.N|RepeatList|否|TestKey|资源的标签键。N的取值范围：1~20 |
|All|Boolean|否|false|是否解绑实例上全部的标签。当请求中未设置TagKey.N时，该参数才有效。取值范围：

 -   true
-   false

 默认值：false |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5414A4E5-4C36-4461-95FC-23757A20B5F8|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=UntagResources
&RegionId=cn-hangzhou
&ResourceId.1=gp-xxxxxxxxxxx
&ResourceType=instance
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>5414A4E5-4C36-4461-95FC-23757A20B5F8</RequestId>
```

`JSON` 格式

```
{
    "RequestId": "5414A4E5-4C36-4461-95FC-23757A20B5F8"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

