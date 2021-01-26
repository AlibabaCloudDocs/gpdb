# ModifySQLCollectorPolicy

调用ModifySQLCollectorPolicy开启或关闭指定实例的SQL采集功能。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ModifySQLCollectorPolicy&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifySQLCollectorPolicy|系统规定参数。取值：ModifySQLCollectorPolicy。 |
|DBInstanceId|String|是|gp-xxxxxxxx|实例ID。 |
|SQLCollectorStatus|String|是|Enable|SQL采集状态。

 -   Enable：SQL采集开启
-   Disabled：SQL采集关闭 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|4FA1F1D1-50A6-4F60-9A78-5752F2076A53|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=ModifySQLCollectorPolicy
&DBInstanceId=gp-xxxxxxxx
&SQLCollectorStatus=Enable
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ModifySQLCollectorPolicyResponse>
           <RequestId>4FA1F1D1-50A6-4F60-9A78-5752F2076A53</RequestId>
</ModifySQLCollectorPolicyResponse>
```

`JSON` 格式

```
{
  "RequestId":"4FA1F1D1-50A6-4F60-9A78-5752F2076A53"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

