# ModifyDBInstanceSecurityIps

调取ModifyDBInstanceSecurityIps接口修改实例的白名单列表。

****

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceSecurityIps&type=RPC&version=2019-06-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyDBInstanceSecurityIps|系统规定参数。取值：ModifyDBInstanceSecurityIps。 |
|AliUid|Long|是|123456789123456|阿里云账号ID。 |
|GroupName|String|是|WhileListGroupName|白名单的分组名。 |
|InstanceId|String|是|gp-xxxxxxxxxxx|实例ID。 |
|WhileList|String|是|"127.0.0.1,192.168.0.1"|白名单列表，多个用逗号分隔。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|"200"|内部错误码信息。 |
|HttpStatusCode|Integer|200|通用http错误码。 |
|Message|String|Successful|接口返回详细信息。 |
|RequestId|String|D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD|请求ID。 |
|Success|Boolean|true|是否成功。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com//?Action=ModifyDBInstanceSecurityIps
&AliUid=123456789123456
&GroupName=WhileListGroupName
&InstanceId=gp-xxxxxxxxxxx
&WhileList="127.0.0.1,192.168.0.1"
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<Message>Successful</Message>
<RequestId>API-83e15d2d-86a8-474f-af96-4fbb81272471</RequestId>
<HttpStatusCode>200</HttpStatusCode>
<Code>200</Code>
<Success>true</Success>
```

`JSON`格式

```
{
  "Message": "Successful",
  "RequestId": "D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD",
  "HttpStatusCode": 200,
  "Code": "200",
  "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

