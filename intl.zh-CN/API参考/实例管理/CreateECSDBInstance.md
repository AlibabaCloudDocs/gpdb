# CreateECSDBInstance

调用CreateECSDBInstance接口创建ECS形态的AnalyticDB PostgreSQL版实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=CreateECSDBInstance&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateECSDBInstance|系统规定参数。取值：CreateECSDBInstance。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |
|ZoneId|String|是|cn-hangzhou|可用区ID。如cn-hangzhou-d，可选的可用区详见DescribeRegions接口。 |
|EngineVersion|String|是|4.3|引擎版本，取值为4.3。 |
|Engine|String|是|gpdb|引擎，取值为gpdb。 |
|InstanceSpec|String|是|gpdb.group.segsdx1|实例规格，详见[实例规格表](~~86942~~)。 |
|SegNodeNum|Integer|是|4|节点数量。 |
|SegStorageType|String|是|ESSD|存储磁盘类型。 |
|StorageSize|Integer|是|200|存储空间大小，单位GB。 |
|DBInstanceDescription|String|否|gp-xxxxxxxxxx|AnalyticDB for PostgreSQL实例描述，长度为不超过256字符。 |
|SecurityIPList|String|否|127.0.0.1|IP白名单，默认值为`127.0.0.1`。 |
|PayType|String|否|Prepaid|计费类型：

 -   Postpaid：按量付费
-   Prepaid：包年包月 |
|Period|String|否|Month|购买资源的时长单位。 |
|UsedTime|String|否|1|购买资源的时长。 |
|ClientToken|String|否|0c593ea1-3bea-11e9-b96b-88e9fe637760|幂等性校验。 |
|InstanceNetworkType|String|否|VPC|实例网络类型，取值为**VPC**。 |
|VPCId|String|否|vpc-xxxxxxxxxx|InstanceNetworkType=VPC时，VPCId 必须填写，VPCId所在地域必须与RegionId 保持一致。 |
|VSwitchId|String|否|vsw-xxxxxxxxx|InstanceNetworkType=VPC时，VSwitchId 必须填写，VSwitchId所在可用区必须与ZoneId 保持一致。 |
|PrivateIpAddress|String|否|127.0.0.1|私有地址。 |
|EncryptionKey|String|否|2axxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|如果参数EncryptionType启用加密则通过该参数指定同地域内加密的密钥ID，否则为空。 |
|EncryptionType|String|否|NULL|加密类型，可选项为：

 -   NULL：不启用加密（默认值）
-   CloudDisk：云盘加密并通过EncryptionKey参数指定密钥

 注意：当前云盘加密开启后无法关闭。 |
|MasterNodeNum|Integer|否|cn-hangzhou|地域ID。 |
|SrcDbInstanceName|String|否|cn-hangzhou|可用区ID。如cn-hangzhou-d，可选的可用区详见DescribeRegions接口。 |
|BackupId|String|否|4.3|引擎版本，取值为4.3。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|5414A4E5-4C36-4461-95FC-23757A20B5F8|请求ID。 |
|DBInstanceId|String|gp-xxxxxxxxx|实例ID。 |
|Port|String|3432|端口。 |
|ConnectionString|String|gp-xxxxxxx.gpdb.rds.aliyuncs.com|连接地址。 |
|OrderId|String|xxxxxxxx|订单编号。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=CreateECSDBInstance
&ClientToken=0c593ea1-3bea-11e9-b96b-88e9fe637760
&Engine=gpdb
&EngineVersion=4.3
&InstanceSpec=gpdb.group.segsdx1
&RegionId=cn-hangzhou
&SecurityIPList=127.0.0.1
&SegNodeNum=4
&SegStorageType=ESSD
&StorageSize=200
&ZoneId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML`格式

```
HTTP/1.1 200 OK
Content-Type:application/xml

<RequestId>5414A4E5-4C36-4461-95FC-23757A20B5F8</RequestId>
<DBInstanceId>gp-xxxxxxxxx</DBInstanceId>
<Port>3432</Port>
<OrderId>xxxxxxxx</OrderId>
<ConnectionString>gp-xxxxxxx.gpdb.rds.aliyuncs.com</ConnectionString>
```

`JSON`格式

```
HTTP/1.1 200 OK
Content-Type:application/json

{
  "RequestId" : "5414A4E5-4C36-4461-95FC-23757A20B5F8",
  "DBInstanceId" : "gp-xxxxxxxxx",
  "Port" : "3432",
  "OrderId" : "xxxxxxxx",
  "ConnectionString" : "gp-xxxxxxx.gpdb.rds.aliyuncs.com"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

