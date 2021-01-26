# ModifyDBInstanceNetworkType

调用ModifyDBInstanceNetworkType将实例的网络类型从VPC切换成经典网络，或从经典网络切换成VPC。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceNetworkType&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|DBInstanceId|String|是|gp-xxxxxxxx|实例ID。 |
|InstanceNetworkType|String|是|VPC|指定网络类型：

 -   VPC：专有网络类型
-   Classic：经典网络类型 |
|VPCId|String|否|vpc-xxxxxxxx|VPC ID。 |
|VSwitchId|String|否|vsw-xxxxxxxx|VSwitch ID，若传入VPCId的值，则该参数必传。 |
|PrivateIpAddress|String|否|192.168.0.10|您可以指定VSwitchId下的VPC IP。如果不输入，系统通过VPCId和VSwitchId自动分配。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|2d0c35a9-f5da-44ba-852d-741e27b7eb0b|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceNetworkType
&DBInstanceId=gp-xxxxxxxx
&InstanceNetworkType=VPC
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ModifyDBInstanceNetworkTypeResponse>
           <RequestId>2d0c35a9-f5da-44ba-852d-741e27b7eb0b</RequestId>
</ModifyDBInstanceNetworkTypeResponse>
```

`JSON` 格式

```
{
  "RequestId":"2d0c35a9-f5da-44ba-852d-741e27b7eb0b"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

