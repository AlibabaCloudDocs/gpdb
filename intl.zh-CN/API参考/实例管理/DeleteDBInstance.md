# DeleteDBInstance

调用DeleteDBInstance释放按量付费实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DeleteDBInstance&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DeleteDBInstance|系统规定参数。取值：DeleteDBInstance。 |
|DBInstanceId|String|是|gp-xxxxxx|实例名。 |
|ClientToken|String|否|0c593ea1-3bea-11e9-b96b-88e9fe637760|保证请求幂等性。从您的客户端生成一个参数值，确保不同请求间该参数值唯一。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|65BDA532-28AF-4122-AA39-B382721EEE64|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DeleteDBInstance
&DBInstanceId=gp-xxxxxx
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DeleteDBInstanceResponse>  
       <RequestId>65BDA532-28AF-4122-AA39-B382721EEE64</RequestId>
</DeleteDBInstanceResponse>
```

`JSON` 格式

```
{
    "RequestId": "65BDA532-28AF-4122-AA39-B382721EEE64"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

