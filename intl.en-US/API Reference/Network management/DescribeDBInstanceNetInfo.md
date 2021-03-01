# DescribeDBInstanceNetInfo

You can call this operation to query the connection information of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceNetInfo&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|DBInstanceId|String|Yes|gp-xxxxxxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|The ID of the request. |
|InstanceNetworkType|String|Classic|The network type of the instance. Valid values:

 -   Classic
-   VPC |
|DBInstanceNetInfos|Array| |The connection information of the instance. |
|ConnectionString|String|gp-xxxxxxx.gpdb.rds.aliyuncs.com|The URL of the database. |
|IPAddress|String|127.0.0.1|The IP address. |
|IPType|String|Inner|The type of IP address.

 -   Valid values for instances in the classic network: Inner and Public
-   Valid values for instances in a VPC: Private and Public |
|Port|String|3432|The port information. |
|VPCId|String|vpc-xxxxxxx|The VPC ID of the instance. |
|VSwitchId|String|vsw-xxxxxxxx|The vSwitch ID of the instance. Separate multiple IDs with commas \(,\). |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstanceNetInfo
&DBInstanceId=gp-xxxxxxxxxx
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

