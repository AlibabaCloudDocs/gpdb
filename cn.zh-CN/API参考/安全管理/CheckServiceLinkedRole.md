# CheckServiceLinkedRole

调用CheckServiceLinkedRole接口检查是否创建了SLR。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=CheckServiceLinkedRole&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CheckServiceLinkedRole|系统规定参数。取值：CheckServiceLinkedRole。 |
|RegionId|String|否|cn-hangzhou|地域信息。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|HasServiceLinkedRole|String|true|是否已经创建SLR。 |
|RegionId|String|cn-hangzhou|地域信息。 |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|请求ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=CheckServiceLinkedRole
&<公共请求参数>
&RegionId=cn-hangzhou
```

正常返回示例

`XML`格式

```
<RequestId>54B01BA7-155E-4212-B16A-E761DBD1FA97</RequestId>
<HasServiceLinkedRole>true</HasServiceLinkedRole>
<RegionId>cn-hangzhou</RegionId>
```

`JSON`格式

```
{
  "RequestId": "54B01BA7-155E-4212-B16A-E761DBD1FA97",
  "HasServiceLinkedRole": true,
  "RegionId": "cn-hangzhou"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

