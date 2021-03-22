# UpgradeDBVersion

调用UpgradeDBVersion为指定实例升级内核小版本。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=UpgradeDBVersion&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|UpgradeDBVersion|系统规定参数。取值：UpgradeDBVersion。 |
|DBInstanceId|String|是|gp-wz9kmr708m155j\*\*\*|实例ID。 |
|MajorVersion|String|是|6.0|大版本。 |
|MinorVersion|String|是|6.0|小版本。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |
|SwitchTimeMode|String|否|xxxxx|升级方式。 |
|SwitchTime|String|否|xxxxx|升级时间。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|DBInstanceId|String|gp-wz9kmr708m155j\*\*\*|实例ID。 |
|DBInstanceName|String|gp-wz9kmr708m155j\*\*\*|实例名称。 |
|RequestId|String|25C11EE5-B7E8-481A-A07C-BD619971A570|请求ID。 |
|TaskId|String|101450956|任务ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=UpgradeDBVersion
&DBInstanceId=gp-wz9kmr708m155j***
&MajorVersion=6.0
&MinorVersion=6.0
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<TaskId>101450956</TaskId>
<RequestId>25C11EE5-B7E8-481A-A07C-BD619971A570</RequestId>
<DBInstanceId>gp-wz9kmr708m155j***</DBInstanceId>
<DBInstanceName>gp-wz9kmr708m155j***</DBInstanceName>
```

`JSON`格式

```
{"TaskId":"101450956","RequestId":"25C11EE5-B7E8-481A-A07C-BD619971A570","DBInstanceId":"gp-wz9kmr708m155j***","DBInstanceName":"gp-wz9kmr708m155j***"}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

