# ModifyDBInstanceConnectionString

调用修改实例的内外网连接URL。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceConnectionString&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyDBInstanceConnectionString|系统规定参数。取值：ModifyDBInstanceConnectionString。 |
|ConnectionStringPrefix|String|是|gp-xxxxxxxx|目标连接地址。 |
|CurrentConnectionString|String|是|gp-xxxxxxx.gpdb.rds.aliyuncs.com|实例当前的某个连接地址。 |
|DBInstanceId|String|是|gp-xxxxxxx|实例ID。 |
|Port|String|是|3432|目标端口。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|29B0BF34-D069-4495-92C7-FA6D94528A9E|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceConnectionString
&ConnectionStringPrefix=gp-xxxxxxxx
&CurrentConnectionString=gp-xxxxxxx.gpdb.rds.aliyuncs.com
&DBInstanceId=gp-xxxxxxx
&Port=3432
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<ModifyDBInstanceConnectionStringResponse>
           <RequestId>29B0BF34-D069-4495-92C7-FA6D94528A9E</RequestId>
</ModifyDBInstanceConnectionStringResponse>
```

`JSON`格式

```
{
	"RequestId": "29B0BF34-D069-4495-92C7-FA6D94528A9E"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

