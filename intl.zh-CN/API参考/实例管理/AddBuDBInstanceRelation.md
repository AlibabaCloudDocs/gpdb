# AddBuDBInstanceRelation

调取AddBuDBInstanceRelation接口添加实例BusinessUnit关联关系。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=AddBuDBInstanceRelation&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|AddBuDBInstanceRelation|系统规定参数。取值：AddBuDBInstanceRelation。 |
|BusinessUnit|String|是|BusinessUnit|BU信息。 |
|DBInstanceId|String|是|gp-xxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|BusinessUnit|String|BusinessUnit|BusinessUnit信息。 |
|DBInstanceName|String|gp-xxxxxx|实例ID。 |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|请求ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=AddBuDBInstanceRelation
&DBInstanceId=gp-xxxxxx
&BusinessUnit=BusinessUnit
```

正常返回示例

`XML`格式

```
<RequestId>D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD</RequestId>
<BusinessUnit>BusinessUnit</BusinessUnit>
<DBInstanceName>gp-xxxxxx</DBInstanceName>
```

`JSON`格式

```
{
  "RequestId":"D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD",
  "BusinessUnit": "BusinessUnit",
  "DBInstanceName": "gp-xxxxxx"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

