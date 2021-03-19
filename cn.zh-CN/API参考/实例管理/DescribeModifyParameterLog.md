# DescribeModifyParameterLog

调取DescribeModifyParameterLog接口获取参数修改历史。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeModifyParameterLog&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeModifyParameterLog|系统规定参数。取值：DescribeModifyParameterLog。 |
|DBInstanceId|String|是|gp-xxxxxx|实例ID。 |
|StartTime|String|否|2020-02-02T11:22:22Z|查询起始时间。 |
|EndTime|String|否|2020-05-05T11:22:22Z|查询截止时间。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Changelogs|Array of Changelogs| |变更历史。 |
|EffectTime|String|2020-05-05T11:22:22Z|生效时间。 |
|ParameterName|String|testkey|参数名。 |
|ParameterValid|String|true|是否生效。 |
|ParameterValueAfter|String|100|修改前的参数。 |
|ParameterValueBefore|String|200|修改后的参数。 |
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|请求ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeModifyParameterLog
&<公共请求参数>
&DBInstanceId=gp-xxxxxx
&StartTime=2020-02-02T11:22:22Z
&EndTime=2020-05-05T11:22:22Z
```

正常返回示例

`XML`格式

```
<RequestId>F805CBCF-4BE4-410B-9FF8-41842799E5B8</RequestId>
<Changelogs>
    <ParameterValueBefore>10800000</ParameterValueBefore>
    <EffectTime>2021-03-16T11:48:14Z</EffectTime>
    <ParameterName>statement_timeout</ParameterName>
    <ParameterValueAfter>11800000</ParameterValueAfter>
    <ParameterValid>false</ParameterValid>
</Changelogs>
```

`JSON`格式

```
{
  "RequestId": "F805CBCF-4BE4-410B-9FF8-41842799E5B8",
  "Changelogs": [
    {
      "ParameterValueBefore": "10800000",
      "EffectTime": "2021-03-16T11:48:14Z",
      "ParameterName": "statement_timeout",
      "ParameterValueAfter": "11800000",
      "ParameterValid": "false"
    }
  ]
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

