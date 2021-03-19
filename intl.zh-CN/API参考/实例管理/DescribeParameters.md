# DescribeParameters

调取DescribeParameters接口获取参数信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeParameters&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeParameters|系统规定参数。取值：DescribeParameters。 |
|DBInstanceId|String|是|gp-xxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Parameters|Array of Parameters| |参数信息。 |
|CurrentValue|String|1000|当前参数值。 |
|ForceRestartInstance|String|false|是否需要重启实例。 |
|IsChangeableConfig|String|true|是否是可修改的配置。 |
|OptionalRange|String|\[1-10000\]|参数范围。 |
|ParameterDescription|String|param description|参数描述。 |
|ParameterName|String|testkey|参数名。 |
|ParameterValue|String|800|参数默认值。 |
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|请求ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeParameters
&<公共请求参数>
&DBInstanceId=gp-xxxxxx
```

正常返回示例

`XML`格式

```
<Parameters>
    <OptionalRange>[0-2147483647]</OptionalRange>
    <ParameterValue>10800000</ParameterValue>
    <IsChangeableConfig>true</IsChangeableConfig>
    <CurrentValue>11800000</CurrentValue>
    <ParameterName>statement_timeout</ParameterName>
    <ParameterDescription>Sets the maximum allowed duration of any statement，A value of 0 turns off the timeout.</ParameterDescription>
    <ForceRestartInstance>false</ForceRestartInstance>
</Parameters>
<Parameters>
    <OptionalRange>[multi_write_ec|multi_write_sc|single|multi_read]</OptionalRange>
    <ParameterValue>single</ParameterValue>
    <IsChangeableConfig>true</IsChangeableConfig>
    <CurrentValue>single</CurrentValue>
    <ParameterName>rds_master_mode</ParameterName>
    <ParameterDescription>Enable global strong consistency when rds_master_mode is set to multi_write_sc, and global eventual consistency when rds_master_mode is set to multi_write_ec</ParameterDescription>
    <ForceRestartInstance>true</ForceRestartInstance>
</Parameters>
<RequestId>8CA0F95F-0F2D-4E85-ADD1-32C7FCE5E17D</RequestId>
```

`JSON`格式

```
{
  "Parameters": [
    {
      "OptionalRange": "[0-2147483647]",
      "ParameterValue": "10800000",
      "IsChangeableConfig": "true",
      "CurrentValue": "11800000",
      "ParameterName": "statement_timeout",
      "ParameterDescription": "Sets the maximum allowed duration of any statement，A value of 0 turns off the timeout.",
      "ForceRestartInstance": "false"
    },
    {
      "OptionalRange": "[multi_write_ec|multi_write_sc|single|multi_read]",
      "ParameterValue": "single",
      "IsChangeableConfig": "true",
      "CurrentValue": "single",
      "ParameterName": "rds_master_mode",
      "ParameterDescription": "Enable global strong consistency when rds_master_mode is set to multi_write_sc, and global eventual consistency when rds_master_mode is set to multi_write_ec",
      "ForceRestartInstance": "true"
    }
  ],
  "RequestId": "8CA0F95F-0F2D-4E85-ADD1-32C7FCE5E17D"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

