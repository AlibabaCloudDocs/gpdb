# ModifySecurityIps

调用ModifySecurityIps修改允许访问实例的IP名单。

接口调用必须满足以下条件，否则将失败：

-   实例运行中
-   实例没有被锁定

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ModifySecurityIps&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifySecurityIps|系统规定参数。取值：ModifySecurityIps。 |
|DBInstanceId|String|是|gp-xxxxxxxxx|实例ID。 |
|SecurityIPList|String|是|127.0.0.1|IP白名单分组下的IP列表，最多1000个，以逗号隔开，格式如下：

 -   0.0.0.0/0
-   10.23.12.24（IP）
-   10.23.12.24/24（CIDR模式，无类域间路由，`/24`表示地址中前缀的长度，范围为`[1,32]`） |
|DBInstanceIPArrayName|String|否|Default|IP白名单分组的名字，如果不传默认操作“Default”分组。

 **说明：** 1个实例最多支持50个白名单分组。 |
|DBInstanceIPArrayAttribute|String|否|hidden|默认为空。用于区分不同的属性值，控制台不显示带有`hidden`属性的分组。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|871C698F-B43D-4D1D-ACD6-DF56B0F89978|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=ModifySecurityIps
&DBInstanceId=gp-xxxxxxxxx
&SecurityIPList=127.0.0.1
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ModifySecurityIpsResponse>
           <RequestId>871C698F-B43D-4D1D-ACD6-DF56B0F89978</RequestId>
</ModifySecurityIpsResponse>
```

`JSON` 格式

```
{
  "RequestId":"871C698F-B43D-4D1D-ACD6-DF56B0F89978"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

