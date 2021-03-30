# DescribeDBInstanceAttribute

You can call this operation to query the details of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceAttribute&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeDBInstanceAttribute|The operation that you want to perform. Set the value to DescribeDBInstanceAttribute. |
|DBInstanceId|String|Yes|gp-xxxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Items|Array of DBInstanceAttribute| |Details about the instance. |
|DBInstanceAttribute| | | |
|AvailabilityValue|String|100.0%|The service availability of the current instance. Unit: %. |
|ConnectionMode|String|Performance|The access mode of the instance.

 -   Performance: standard mode
-   Safety: safe mode |
|ConnectionString|String|gp-xxxxxxx.gpdb.rds.aliyuncs.com|The endpoint that is used to connect to the specified database. |
|CpuCoresPerNode|Integer|2|The number of CPU cores per node. |
|CreationTime|String|2018-06-27T12:07:10Z|The time when the instance was created. The time is in the `YYYY-MM-DDThh:mm:ssZ` format. Example: 2019-05-30T12:11:4Z. |
|DBInstanceClass|String|gpdb.group.segsdx2|The specifications of the instance. |
|DBInstanceClassType|String|x|The instance family. Valid values:

 -   s: shared instance
-   x: general-purpose instance
-   d: dedicated instance
-   h: dedicated host instance |
|DBInstanceCpuCores|Integer|2|The number of CPU cores of the instance. |
|DBInstanceDescription|String|gp-xxxxxxx|The description of the instance. |
|DBInstanceDiskMBPS|Long|300|The maximum disk throughput of a compute node. Unit: Mbit/s. |
|DBInstanceGroupCount|String|2|The number of compute nodes. |
|DBInstanceId|String|gp-xxxxxxx|The ID of the instance. |
|DBInstanceMemory|Long|16384|The memory capacity of the instance. Unit: MB. |
|DBInstanceNetType|String|Internet|The type of the network interface card \(NIC\) that is used by the instance.

 -   Internet
-   Intranet |
|DBInstanceStatus|String|Running|The state of the instance. For more information, see [Instance statuses](~~86944~~). |
|DBInstanceStorage|Long|160|The disk space of the instance. Unit: GB. |
|Engine|String|gpdb|The database engine of the instance |
|EngineVersion|String|4.3|The version of the database engine. |
|ExpireTime|String|2019-06-27T16:00:00Z|The expiration time of the instance. |
|HostType|String|1|The disk type of compute nodes. Valid values:

 -   0: SSD
-   1: HDD |
|InstanceNetworkType|String|VPC|The network type of the instance. Valid values:

 -   Classic
-   VPC |
|LockMode|String|Unlock|The locking mode of the instance. Valid values:

 -   Unlock: The instance is not locked.
-   ManualLock: The instance is manually locked.
-   LockByExpiration: The instance is automatically locked after the instance expires.
-   LockByRestoration: The instance is automatically locked before a rollback.
-   LockByDiskQuota: The instance is automatically locked when the disk space is full. |
|LockReason|String|Unknow|The reason why the instance was locked. |
|MaintainEndTime|String|22:00Z|The end time of the maintenance window. |
|MaintainStartTime|String|18:00Z|The start time of the maintenance window. |
|MaxConnections|Integer|500|The maximum number of concurrent connections to an instance. |
|MemoryPerNode|Integer|1664|The memory capacity per node. |
|MemoryUnit|String|MB|The unit of the memory capacity. |
|PayType|String|Prepaid|The billing method of the instance. Valid values:

 -   Postpaid: pay-as-you-go
-   Prepaid: subscription |
|Port|String|3432|The port used to connect to the instance. |
|ReadDelayTime|String|2|The read latency of the instance. |
|RegionId|String|cn-hangzhou-e|The region ID of the instance. |
|SecurityIPList|String|127.0.0.1|The list of one or more IP addresses that are allowed to access the instance. |
|SegmentCounts|Integer|2|The number of compute nodes. |
|StoragePerNode|Integer|1664|The disk space per node. |
|StorageType|String|HDD|The type of the disk. |
|StorageUnit|String|MB|The unit of the disk space. |
|Tags|Array of Tag| |The list of one or more tags. Each tag consists of a key-value pair. |
|Tag| | | |
|Key|String|key1|The tag key of the instance. |
|Value|String|value1|The tag value of the instance. |
|VpcId|String|vpc-xxxxxxxx|The ID of the VPC for which the ClassicLink feature is enabled. |
|ZoneId|String|cn-hangzhou|The zone ID of the instance. |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstanceAttribute
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>
```

Sample success responses

`XML` format

```
<Items>
    <DBInstanceAttribute>
        <DBInstanceDiskMBPS>300</DBInstanceDiskMBPS>
        <ConnectionString>gp-xxxxxxx.gpdb.rds.aliyuncs.com</ConnectionString>
        <ZoneId>cn-hangzhou-e</ZoneId>
        <ConnectionMode>Performance</ConnectionMode>
        <VpcId>vpc-xxxxxxxx</VpcId>
        <Engine>gpdb</Engine>
        <MaxConnections>500</MaxConnections>
        <MaintainStartTime>18:00Z</MaintainStartTime>
        <DBInstanceGroupCount>2</DBInstanceGroupCount>
        <HostType>0</HostType>
        <DBInstanceMemory>16384</DBInstanceMemory>
        <EngineVersion>4.3</EngineVersion>
        <DBInstanceStatus>Running</DBInstanceStatus>
        <PayType>Prepaid</PayType>
        <LockMode>Unlock</LockMode>
        <DBInstanceNetType>1</DBInstanceNetType>
        <MaintainEndTime>22:00Z</MaintainEndTime>
        <DBInstanceClass>gpdb.group.segsdx2</DBInstanceClass>
        <DBInstanceId>gp-xxxxxxx</DBInstanceId>
        <DBInstanceClassType>x</DBInstanceClassType>
        <InstanceNetworkType>Classic</InstanceNetworkType>
        <DBInstanceDescription>gp-xxxxxxx</DBInstanceDescription>
        <DBInstanceStorage>160</DBInstanceStorage>
        <CreationTime>2018-06-27T12:07:10Z</CreationTime>
        <Port>3432</Port>
        <ExpireTime>2019-06-27T16:00:00Z</ExpireTime>
        <RegionId>cn-hangzhou</RegionId>
        <AvailabilityValue>100.0%</AvailabilityValue>
        <DBInstanceCpuCores>2</DBInstanceCpuCores>
        <SecurityIPList>127.0.0.1</SecurityIPList>
    </DBInstanceAttribute>
</Items>
<RequestId>B4CAF581-2AC7-41AD-8940-D56DF7AADF5B</RequestId>
```

`JSON` format

```
{
    "Items":{
        "DBInstanceAttribute":[
            {
                "DBInstanceDiskMBPS":300,
                "ConnectionString":"gp-xxxxxxx.gpdb.rds.aliyuncs.com",
                "ZoneId":"cn-hangzhou-e",
                "ConnectionMode":"Performance",
                "VpcId":"vpc-xxxxxxxx",
                "Engine":"gpdb",
                "MaxConnections":500,
                "MaintainStartTime":"18:00Z",
                "DBInstanceGroupCount":"2",
                "HostType":"0",
                "DBInstanceMemory":16384,
                "EngineVersion":"4.3",
                "DBInstanceStatus":"Running",
                "PayType":"Prepaid",
                "LockMode":"Unlock",
                "DBInstanceNetType":"1",
                "MaintainEndTime":"22:00Z",
                "DBInstanceClass":"gpdb.group.segsdx2",
                "DBInstanceId":"gp-xxxxxxx",
                "DBInstanceClassType":"x",
                "InstanceNetworkType":"Classic",
                "DBInstanceDescription":"gp-xxxxxxx",
                "DBInstanceStorage":160,
                "CreationTime":"2018-06-27T12:07:10Z",
                "Port":"3432",
                "ExpireTime":"2019-06-27T16:00:00Z",
                "RegionId":"cn-hangzhou",
                "AvailabilityValue":"100.0%",
                "DBInstanceCpuCores":2,
                "SecurityIPList":"127.0.0.1"
            }
        ]
    },
    "RequestId":"B4CAF581-2AC7-41AD-8940-D56DF7AADF5B"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

