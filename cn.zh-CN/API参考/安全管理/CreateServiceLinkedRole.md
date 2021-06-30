# CreateServiceLinkedRole

调用CreateServiceLinkedRole接口创建服务关联角色（SLR）。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=CreateServiceLinkedRole&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateServiceLinkedRole|系统规定参数。取值：**CreateServiceLinkedRole**。 |
|RegionId|String|是|cn-hangzhou|地域ID，您可以通过[DescribeRegions](~~86912~~)接口查看可用的地域ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|请求ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=CreateServiceLinkedRole
&RegionId=cn-hangzhou
&公共请求参数
```

正常返回示例

`XML`格式

```
HTTP/1.1 200 OK
Content-Type:application/xml

<CreateServiceLinkedRoleResponse>
    <RequestId>B4CAF581-2AC7-41AD-8940-D56DF7AADF5B</RequestId>
</CreateServiceLinkedRoleResponse>
```

`JSON`格式

```
HTTP/1.1 200 OK
Content-Type:application/json

{
  "RequestId" : "B4CAF581-2AC7-41AD-8940-D56DF7AADF5B"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

