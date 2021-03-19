# SwitchDBInstanceNetType

调用SwitchDBInstanceNetType接口切换内外网地址。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=SwitchDBInstanceNetType&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|SwitchDBInstanceNetType|系统规定参数。取值：SwitchDBInstanceNetType。 |
|ConnectionStringPrefix|String|是|test1234|自定义连接地址的前辍：

 -   由小写字母，数字，中划线组成，字母开头；
-   长度不超过30个字符。 |
|DBInstanceId|String|是|rm-uf6wjk5xxxxxxx|实例ID。 |
|Port|String|是|3306|端口号，取值：3001~3999。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|FA67B751-2A2D-470C-850B-D6B93699D35C|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=SwitchDBInstanceNetType
&ConnectionStringPrefix=test1234
&DBInstanceId=rm-uf6wjk5xxxxxxx
&Port=3306
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<RequestId>FA67B751-2A2D-470C-850B-D6B93699D35C</RequestId>
```

`JSON`格式

```
{"RequestId":"FA67B751-2A2D-470C-850B-D6B93699D35C"}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

