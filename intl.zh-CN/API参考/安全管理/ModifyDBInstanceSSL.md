# ModifyDBInstanceSSL

调用ModifyDBInstanceSSL接口开启，关闭，更新SSL。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceSSL&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyDBInstanceSSL|系统规定参数，取值：ModifyDBInstanceSSL。 |
|ConnectionString|String|是|gp-t4ni14frt6lhjk74a-master.gpdbmaster.singapore.rds.aliyuncs.com|加密的连接串，对于ECS实例，该参数默认采用泛域名，会加密所有的连接串。 |
|DBInstanceId|String|是|gp-xxxxxxxxxxx|实例ID。 |
|SSLEnabled|Integer|是|1|SSL状态：

 -   0:关闭
-   1:开启
-   2:更新 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|ADD6EA90-EECB-4C12-9F26-0B6DB58710EF|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ModifyDBInstanceSSL
&ConnectionString=gp-t4ni14frt6lhjk74a-master.gpdbmaster.singapore.rds.aliyuncs.com
&DBInstanceId=gp-xxxxxxxxxxx
&SSLEnabled=1
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<RequestId>ADD6EA90-EECB-4C12-9F26-0B6DB58710EF</RequestId>
```

`JSON`格式

```
{"RequestId":"ADD6EA90-EECB-4C12-9F26-0B6DB58710EF"}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

