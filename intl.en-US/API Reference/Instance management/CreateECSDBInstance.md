# CreateECSDBInstance

You can call this operation to create an AnalyticDB for PostgreSQL instance that is hosted on Elastic Compute Service \(ECS\) instances.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=CreateECSDBInstance&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateECSDBInstance|The operation that you want to perform. Set the value to CreateECSDBInstance. |
|ClientToken|String|Yes|0c593ea1-3bea-11e9-b96b-88e9fe637760|The client token that is used to ensure the idempotence of the request. |
|Engine|String|Yes|gpdb|The engine of the database. Set the value to gpdb. |
|EngineVersion|String|Yes|4.3|The version of the database engine. Set the value to 4.3. |
|InstanceSpec|String|Yes|gpdb.group.segsdx1|The instance type. For more information, see [Instance types](~~86942~~). |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. |
|SecurityIPList|String|Yes|127.0.0.1|The whitelist of the IP addresses that are allowed to access the instance. Default value: `127.0.0.1`. |
|SegNodeNum|Integer|Yes|4|The number of nodes. |
|SegStorageType|String|Yes|ESSD|The storage type of the disk. |
|StorageSize|Integer|Yes|200|The storage capacity. Unit: GB. |
|ZoneId|String|Yes|cn-hangzhou|The zone ID of the instance. Example: cn-hangzhou-d. You can call the DescribeRegions operation to query available zones. |
|DBInstanceDescription|String|No|gp-xxxxxxxxxx|The description of the instance. The description can be up to 256 characters in length. |
|PayType|String|No|Prepaid|The billing method of the instance. Valid values:

 -   Postpaid: pay-as-you-go
-   Prepaid: subscription |
|Period|String|No|Month|The unit of the subscription period. |
|UsedTime|String|No|1|The subscription period. |
|InstanceNetworkType|String|No|VPC|The network type of the instance. Set the value to **VPC**. |
|VPCId|String|No|vpc-xxxxxxxxxx|The VPC ID of the instance. If you set the InstanceNetworkType parameter to VPC, you must also specify the VPCId parameter. The specified region of the VPC must be the same as the RegionId value. |
|VSwitchId|String|No|vsw-xxxxxxxxx|The vSwitch ID of the instance. If you set the InstanceNetworkType parameter to VPC, you must also specify the VSwitchId parameter. The specified zone of the vSwitchId must be the same as the ZoneId value. |
|PrivateIpAddress|String|No|127.0.0.1|The private IP address of the instance. |
|EncryptionKey|String|No|2axxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|If the EncryptionType parameter is set to CloudDisk, you must specify this parameter to the encryption key that is in the same region with the disks that is specified by the EncryptionType parameter. Otherwise, leave this parameter empty. |
|EncryptionType|String|No|NULL|The type of the encryption. Default value: NULL. Valid values:

 -   NULL: Encryption is disabled.
-   CloudDisk: Encryption is enabled on disks and the encryption key is specified by using the EncryptionKey parameter.

 Note: Disk encryption cannot be disabled after it is enabled. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|ConnectionString|String|gp-xxxxxxx.gpdb.rds.aliyuncs.com|The IP address that is used to connect to the specified database. |
|DBInstanceId|String|gp-xxxxxxxxx|The ID of the instance. |
|OrderId|String|xxxxxxxx|The order ID of the instance. |
|Port|String|3432|The port used to connect to the instance. |
|RequestId|String|5414A4E5-4C36-4461-95FC-23757A20B5F8|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=CreateECSDBInstance
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
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>5414A4E5-4C36-4461-95FC-23757A20B5F8</RequestId>
<DBInstanceId>gp-xxxxxxxxx</DBInstanceId>
<Port>3432</Port>
<OrderId>xxxxxxxx</OrderId>
<ConnectionString>gp-xxxxxxx.gpdb.rds.aliyuncs.com</ConnectionString>
```

`JSON` format

```
{"RequestId":"5414A4E5-4C36-4461-95FC-23757A20B5F8","DBInstanceId":"gp-xxxxxxxxx","Port":"3432","OrderId":"xxxxxxxx","ConnectionString":"gp-xxxxxxx.gpdb.rds.aliyuncs.com"}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

