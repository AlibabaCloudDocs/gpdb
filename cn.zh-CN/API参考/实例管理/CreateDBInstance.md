# CreateDBInstance

调用CreateDBInstance接口创建 AnalyticDB for PostgreSQL实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=CreateDBInstance&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateDBInstance|系统规定参数。取值：**CreateDBInstance**。 |
|ClientToken|String|是|0c593ea1-3bea-11e9-b96b-88e9fe637760|幂等性校验。 |
|DBInstanceClass|String|是|gpdb.group.segsdx1|实例规格，详见[实例规格表](~~86942~~)。 |
|DBInstanceGroupCount|String|是|2|AnalyticDB for PostgreSQL 计算组的数量。 |
|Engine|String|是|gpdb|引擎，取值为gpdb。 |
|EngineVersion|String|是|4.3|引擎版本，取值为4.3。 |
|RegionId|String|是|cn-hangzhou|地域ID，如cn-hangzhou，可选的地域详见DescribeRegions接口。 |
|SecurityIPList|String|是|127.0.0.1|IP白名单，默认值为"`127.0.0.1`"。 |
|ZoneId|String|是|cn-hangzhou-i|可用区ID，如cn-hangzhou-d，可选的可用区详见DescribeRegions接口。 |
|DBInstanceDescription|String|否|gp-xxxxxx|AnalyticDB for PostgreSQL实例描述，长度为256字符。 |
|PayType|String|否|Prepaid|付费类型：

 -   Postpaid：按量付费，为默认值。
-   Prepaid：包年包月。 |
|Period|String|否|Month|购买资源的时长单位。 |
|UsedTime|String|否|1|购买资源的时长。 |
|InstanceNetworkType|String|否|VPC|实例网络类型，取值为**VPC**。 |
|VPCId|String|否|vpc-xxxxxxx|InstanceNetworkType=VPC时，VPCId 必须填写，VPCId所在地域必须与RegionId 保持一致。 |
|VSwitchId|String|否|vsw-xxxxxxxx|InstanceNetworkType=VPC时，VSwitchId 必须填写，VSwitchId所在可用区必须与ZoneId 保持一致。 |
|PrivateIpAddress|String|否|127.0.0.1|私有地址。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|ConnectionString|String|gp-xxxxxxx.gpdb.rds.aliyuncs.com|连接地址。 |
|DBInstanceId|String|gp-xxxxxxxxx|实例ID。 |
|OrderId|String|xxxxxxxx|订单编号。 |
|Port|String|3432|端口。 |
|RequestId|String|5414A4E5-4C36-4461-95FC-23757A20B5F8|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=CreateDBInstance
&ClientToken=0c593ea1-3bea-11e9-b96b-88e9fe637760
&Engine=gpdb
&EngineVersion=4.3
&RegionId=cn-hangzhou
&SecurityIPList=127.0.0.1
&ZoneId=cn-hangzhou-i
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<CreateDBInstanceResponse>
          <dbInstanceId>gp-xxxxxxx</dbInstanceId>
          <orderId>xxxxxxxx</orderId>
          <connectionString>gp-xxxxxxx.gpdb.rds.aliyuncs.com</connectionString>
          <port>3432</port>
          <RequestId>5414A4E5-4C36-4461-95FC-23757A20B5F8</RequestId>
</CreateDBInstanceResponse>
```

`JSON` 格式

```
{
	"dbInstanceId": "gp-xxxxxxx",
	"orderId": "xxxxxxxx",
	"connectionString": "gp-xxxxxxx.gpdb.rds.aliyuncs.com",
	"port": "3432",
	"RequestId": "5414A4E5-4C36-4461-95FC-23757A20B5F8"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

