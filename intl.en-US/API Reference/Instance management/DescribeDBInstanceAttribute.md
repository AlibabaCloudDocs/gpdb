# DescribeDBInstanceAttribute

## Description

Queries the details of an instance.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to DescribeDBInstanceAttribute.|
|DBInstanceId|String|Yes|The ID of the instance.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|
|Items|List<DBInstanceAttribute\>|The details of the instance.|

|Parameter|Type|Description|
|---------|----|-----------|
|AvailabilityValue|String|The availability of the instance. Unit: %.|
|ConnectionMode|String|The access mode of the instance. Valid values: -   Performance: standard mode
-   Safety: safe mode |
|ConnectionString|String|The endpoint of the instance.|
|CreationTime|String|The time when the instance was created. The time is in the `YYYY-MM-DDThh:mm:ssZ` format, such as 2011-05-30T12:11:4Z.|
|DBInstanceClass|String|The instance type.|
|DBInstanceClassType|String|The instance family. Valid values: -   s: shared instance
-   x: general-purpose instance
-   d: dedicated instance
-   h: dedicated host instance |
|DBInstanceCpuCores|String|The number of CPU cores.|
|DBInstanceDescription|String|The description of the instance.|
|DBInstanceDiskMBPS|Long|The maximum disk throughput of compute groups. Unit: Mbit/s.|
|DBInstanceGroupCount|String|The number of compute groups.|
|DBInstanceId|String|The ID of the instance.|
|DBInstanceMemory|Long|The memory capacity of the instance. Unit: MB.|
|DBInstanceNetType|String|The type of the network over which you access the instance. Valid values: -   Internet: the Internet
-   Intranet: internal network |
|DBInstanceStatus|String|The status of the instance. For more information, see [Instance statuses](/intl.en-US/API Reference/Appendix/Instance statuses.md).|
|DBInstanceStorage|Long|The disk space of the instance. Unit: GB.|
|Engine|String|The database engine used by the instance.|
|EngineVersion|String|The version of the database engine used by the instance.|
|ExpireTime|String|The expiration time of the instance.|
|HostType|String|The disk type of compute groups. Valid values: -   0: SSD
-   1: SATA |
|InstanceNetworkType|String|The network type of the instance. Valid values: -   Classic: classic network
-   VPC: Virtual Private Cloud |
|LockMode|String|The locking mode of the instance. Valid values: -   Unlock: The instance is not locked.
-   ManualLock: The instance is locked manually.
-   LockByExpiration: The instance is locked automatically after it expires.
-   LockByRestoration: The instance is locked automatically before a rollback.
-   LockByDiskQuota: The instance is locked automatically if its space is used up. |
|MaintainEndTime|String|The end time of the maintenance window for the instance.|
|MaintainStartTime|String|The start time of the maintenance window for the instance.|
|MaxConnections|Integer|The maximum number of concurrent connections to the instance.|
|PayType|String|The billing method of the instance. Valid values: -   Postpaid: pay-as-you-go
-   Prepaid: subscription |
|Port|String|The port of the instance.|
|RegionId|String|The region ID of the instance.|
|SecurityIPList|String|The whitelist of IP addresses that are allowed to access the instance.|
|VpcId|String|The ID of the VPC where ClassicLink is enabled.|
|ZoneId|String|The zone ID of the instance.|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstanceAttribute
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>
			
```

**Sample responses**

**XML format**

```
<DescribeDBInstanceAttributeResponse>
  <RequestId>B4CAF581-2AC7-41AD-8940-D56DF7AADF5B</RequestId>
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
            <DBInstanceDescription>gp-xxxxxxx/DBInstanceDescription>
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
</DescribeDBInstanceAttributeResponse>
```

**JSON format**

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

