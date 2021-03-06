# ModifyDBInstanceDescription

调用ModifyDBInstanceDescription修改实例的备注名。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceDescription&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|DBInstanceDescription|String|是|gp-xxxxxxxx|实例备注信息。 |
|DBInstanceId|String|是|gp-xxxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|107BE202-D1A2-479E-98E0-A892B472562F|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceDescription
&DBInstanceDescription=gp-xxxxxxxx
&DBInstanceId=gp-xxxxxxxx
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ModifyDBInstanceDescriptionResponse>
           <RequestId>107BE202-D1A2-479E-98E0-A892B472562F</RequestId>
</ModifyDBInstanceDescriptionResponse>
```

`JSON` 格式

```
{
        "RequestId": "107BE202-D1A2-479E-98E0-A892B472562F"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

