# DescribeTags

调取DescribeTags接口查询实例标签。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeTags&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|否|DescribeTags|系统默认参数，取值：DescribeTags。 |
|RegionId|String|是|ap-southeast-1|地域信息。 |
|ResourceType|String|是|instance|资源类型。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|A29EC547-B392-4340-AA4F-7C0A7B626E74|请求ID。 |
|Tags|Array of Tag| |标签列表。 |
|TagKey|String|user|标签key。 |
|TagValue|String|test|标签value。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeTags
&RegionId=ap-southeast-1
&ResourceType=instance
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<RequestId>A29EC547-B392-4340-AA4F-7C0A7B626E74</RequestId>
<Tags>
    <TagKey>user</TagKey>
    <TagValue>test</TagValue>
</Tags>
```

`JSON`格式

```
{"RequestId":"A29EC547-B392-4340-AA4F-7C0A7B626E74","Tags":[{"TagKey":"user","TagValue":"test"}]}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

