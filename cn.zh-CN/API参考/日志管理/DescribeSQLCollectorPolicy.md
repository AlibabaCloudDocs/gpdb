# DescribeSQLCollectorPolicy

调用DescribeSQLCollectorPolicy查询指定实例的SQL采集功能是否打开。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeSQLCollectorPolicy&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeSQLCollectorPolicy|系统规定参数。取值：DescribeSQLCollectorPolicy。 |
|DBInstanceId|String|是|gp-xxxxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|ABB39CC3-4488-4857-905D-2E4A051D0521|请求ID。 |
|SQLCollectorStatus|String|Enable|SQL采集状态。

 -   Enable：SQL采集开启。
-   Disabled：SQL采集关闭。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeSQLCollectorPolicy
&DBInstanceId=gp-xxxxxxxxx
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DescribeSQLCollectorPolicyResponse>
	  <RequestId>ABB39CC3-4488-4857-905D-2E4A051D0521</RequestId>
	  <SQLCollectorStatus>Enable</SQLCollectorStatus>
</DescribeSQLCollectorPolicyResponse>
```

`JSON` 格式

```
{
    "RequestId":"ABB39CC3-4488-4857-905D-2E4A051D0521",
    "SQLCollectorStatus":"Enable"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

