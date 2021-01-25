# AllocateInstancePublicConnection

调用AllocateInstancePublicConnection分配实例外网链接地址。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=AllocateInstancePublicConnection&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|否|AllocateInstancePublicConnection|系统规定参数，取值：AllocateInstancePublicConnection。 |
|ConnectionStringPrefix|String|是|gp-xxxxxxxx|连接地址。 |
|DBInstanceId|String|是|gp-xxxxxxxx|实例ID。 |
|Port|String|是|3432|端口号，范围为3200~3999。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|ADD6EA90-EECB-4C12-9F26-0B6DB58710EF|请求ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=AllocateInstancePublicConnection
&ConnectionStringPrefix=gp-xxxxxxxx
&DBInstanceId=gp-xxxxxxxx
&Port=3432
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<AllocateInstancePublicConnectionResponse>  
       <RequestId>ADD6EA90-EECB-4C12-9F26-0B6DB58710EF</RequestId>
</AllocateInstancePublicConnectionResponse>
```

`JSON` 格式

```
{
   "RequestId": "ADD6EA90-EECB-4C12-9F26-0B6DB58710EF"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

