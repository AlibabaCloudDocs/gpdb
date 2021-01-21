# CreateDBInstance

You can call the CreateDBInstance operation to create an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=CreateDBInstance&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateDBInstance|The operation that you want to perform. Set the value to **CreateDBInstance**. |
|ClientToken|String|Yes|0c593ea1-3bea-11e9-b96b-88e9fe637760|The client token that is used to ensure the idempotence of the request. |
|DBInstanceClass|String|Yes|gpdb.group.segsdx1|The instance type. For more information, see [Instance types](/intl.en-US/API Reference/Appendix/Instance types.md). |
|DBInstanceGroupCount|String|Yes|2|The number of compute nodes in the AnalyticDB for PostgreSQL instance. |
|Engine|String|Yes|gpdb|The database engine that the instance runs. Set the value to gpdb. |
|EngineVersion|String|Yes|4.3|The version of the database engine. Set the value to 4.3. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance, such as cn-hangzhou. You can call the DescribeRegions operation to query the most recent region list. |
|SecurityIPList|String|Yes|127.0.0.1|The IP addresses that are allowed to access the instance. Default value: `127.0.0.1`. |
|ZoneId|String|Yes|cn-hangzhou-i|The ID of the zone, such as cn-hangzhou-d. You can call the DescribeRegions operation to query the most recent region list. |
|DBInstanceDescription|String|No|gp-xxxxxx|The description of the instance. The description cannot exceed 256 characters in length. |
|PayType|String|No|Prepaid|The billing method of the instance. Valid values:

 -   Postpaid: pay-as-you-go. This is the default value.
-   Prepaid: subscription. |
|Period|String|No|Month|The unit of the subscription period. |
|UsedTime|String|No|1|The subscription period. |
|InstanceNetworkType|String|No|VPC|The network type of the instance. Set the value to **VPC**. |
|VPCId|String|No|vpc-xxxxxxx|If you set InstanceNetworkType to VPC, you must also specify the VPCId parameter. The specified region of the VPC must be the same as the RegionId value. |
|VSwitchId|String|No|vsw-xxxxxxxx|If you set InstanceNetworkType to VPC, you must also specify the VSwitchId parameter. The specified zone of the VSwitch must be the same as the ZoneId value. |
|PrivateIpAddress|String|No|127.0.0.1|The private IP address of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|ConnectionString|String|gp-xxxxxxx.gpdb.rds.aliyuncs.com|The endpoint of the instance. |
|DBInstanceId|String|gp-xxxxxxxxx|The ID of the instance. |
|OrderId|String|xxxxxxxx|The ID of the order. |
|Port|String|3432|The port used to connect to the instance. |
|RequestId|String|5414A4E5-4C36-4461-95FC-23757A20B5F8|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=CreateDBInstance
&ClientToken=0c593ea1-3bea-11e9-b96b-88e9fe637760
&Engine=gpdb
&EngineVersion=4.3
&RegionId=cn-hangzhou
&SecurityIPList=127.0.0.1
&ZoneId=cn-hangzhou-i
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreateDBInstanceResponse>
          <dbInstanceId>gp-xxxxxxx</dbInstanceId>
          <orderId>xxxxxxxx</orderId>
          <connectionString>gp-xxxxxxx.gpdb.rds.aliyuncs.com</connectionString>
          <port>3432</port>
          <RequestId>5414A4E5-4C36-4461-95FC-23757A20B5F8</RequestId>
</CreateDBInstanceResponse>
```

`JSON` format

```
{
    "dbInstanceId": "gp-xxxxxxx",
    "orderId": "xxxxxxxx",
    "connectionString": "gp-xxxxxxx.gpdb.rds.aliyuncs.com",
    "port": "3432",
    "RequestId": "5414A4E5-4C36-4461-95FC-23757A20B5F8"
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

