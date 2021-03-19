# ModifyParameters

调取ModifyParameters接口修改实例参数。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ModifyParameters&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyParameters|系统规定参数。取值：ModifyParameters。 |
|DBInstanceId|String|是|gp-xxxxxxxx|实例ID。 |
|Parameters|String|是|\{"statement\_timeout":"11800010"\}|参数列表。 |
|ForceRestartInstance|Boolean|否|false|是否强制重启实例。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|F825857F-ECAF-447D-831A-441D9C8784DD|请求ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=ModifyParameters
&DBInstanceId=gp-xxxxxxxx
&Parameters={"statement_timeout":"11800010"}
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<RequestId>F825857F-ECAF-447D-831A-441D9C8784DD</RequestId>
```

`JSON`格式

```
{"RequestId":"F825857F-ECAF-447D-831A-441D9C8784DD"}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

