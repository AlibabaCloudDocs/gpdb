# DescribeDBInstanceNetInfo

调用DescribeDBInstanceNetInfo查询实例的连接信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceNetInfo&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|DBInstanceId|String|是|gp-xxxxxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|请求ID。 |
|InstanceNetworkType|String|Classic|实例网络类型：

 -   Classic：经典网络。
-   VPC：VPC网络。 |
|DBInstanceNetInfos|Array| |实例的连接信息。 |
|ConnectionString|String|gp-xxxxxxx.gpdb.rds.aliyuncs.com|数据库连接URL。 |
|IPAddress|String|127.0.0.1|IP地址。 |
|IPType|String|Inner|IP类型。

 -   经典网络类型的实例IPType为：Inner、Public。
-   VPC类型的实例IPType为：Private、Public。 |
|Port|String|3432|端口信息。 |
|VPCId|String|vpc-xxxxxxx|VPC ID。 |
|VSwitchId|String|vsw-xxxxxxxx|VSwitch ID，多个值用英文逗号“,”隔开。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstanceNetInfo
&DBInstanceId=gp-xxxxxxxxxx
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DescribeDBInstanceNetInfoResponse>
  <instanceNetworkType>Classic</instanceNetworkType>
  <DBInstanceNetInfos>
        <DBInstanceNetInfo>
              <DBInstanceNetType>1</DBInstanceNetType>
              <connectionString>gp-xxxxxxx.gpdb.rds.aliyuncs.com</connectionString>
              <ipAddress>127.0.0.1</ipAddress>
              <port>3432</port>
              <VPCId>vpc-xxxxxxxxx</VPCId>
              <VSwitchId>vsw-xxxxxxx</VSwitchId>
        </DBInstanceNetInfo>
  </DBInstanceNetInfos>
  <RequestId>7565770E-7C45-462D-BA4A-8A5396F2CAD1</RequestId>
</DescribeDBInstanceNetInfoResponse>
```

`JSON` 格式

```
{
	"instanceNetworkType": "Classic",
	"DBInstanceNetInfos": {
		"DBInstanceNetInfo": [
			{
				"DBInstanceNetType": "1",
				"connectionString": "gp-xxxxxxx.gpdb.rds.aliyuncs.com",
				"ipAddress": "127.0.0.1",
				"port": "3432",
				"VPCId": "vpc-xxxxxxxxx",
				"VSwitchId": "vsw-xxxxxxx"
			}
		]
	},
	"RequestId": "7565770E-7C45-462D-BA4A-8A5396F2CAD1"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

