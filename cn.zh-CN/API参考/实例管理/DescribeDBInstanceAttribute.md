# DescribeDBInstanceAttribute

调用DescribeDBInstanceAttribute查询实例详情。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceAttribute&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeDBInstanceAttribute|系统规定参数。取值：DescribeDBInstanceAttribute。 |
|DBInstanceId|String|是|gp-xxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Items|Array of DBInstanceAttribute| |实例详情列表。 |
|DBInstanceAttribute| | | |
|AvailabilityValue|String|100.0%|查询当前实例可用性状态，单位：百分比。 |
|ConnectionMode|String|Performance|访问模式。

 -   Performance：标准访问模式。
-   Safty：高安全访问模式。 |
|ConnectionString|String|gp-xxxxxxx.gpdb.rds.aliyuncs.com|连接地址。 |
|CpuCoresPerNode|Integer|2|单副本的 CPU 核数。 |
|CreationTime|String|2018-06-27T12:07:10Z|创建时间，格式：`YYYY-MM-DDThh:mm:ssZ`，如2019-05-30T12:11:4Z。 |
|DBInstanceClass|String|gpdb.group.segsdx2|实例规格。 |
|DBInstanceClassType|String|x|实例规格族：

 -   s：共享型
-   x：通用型
-   d：独享套餐
-   h：独占物理机 |
|DBInstanceCpuCores|Integer|2|CPU核数。 |
|DBInstanceDescription|String|gp-xxxxxxx|实例描述。 |
|DBInstanceDiskMBPS|Long|300|Group最大的BPS（磁盘吞吐量），单位：Mbps。 |
|DBInstanceGroupCount|String|2|计算组Group数量。 |
|DBInstanceId|String|gp-xxxxxxx|实例ID。 |
|DBInstanceMemory|Long|16384|实例内存，单位：MB。 |
|DBInstanceNetType|String|Internet|实例网卡类型。

 -   Internet：外网
-   Intranet：内网 |
|DBInstanceStatus|String|Running|实例状态，详见[实例状态表](~~86944~~)。 |
|DBInstanceStorage|Long|160|实例存储空间，单位：GB。 |
|Engine|String|gpdb|数据库引擎。 |
|EngineVersion|String|4.3|数据库版本。 |
|ExpireTime|String|2019-06-27T16:00:00Z|过期时间。 |
|HostType|String|1|计算组机器类型取值：

 -   0：SSD
-   1：HDD |
|InstanceNetworkType|String|VPC|实例网络类型。

 -   Classic：经典网络
-   VPC：VPC网络 |
|LockMode|String|Unlock|锁定方式。

 -   Unlock：正常
-   ManualLock：手动触发锁定
-   LockByExpiration：实例过期自动锁定
-   LockByRestoration：实例回滚前的自动锁定
-   LockByDiskQuota：实例空间满自动锁定 |
|LockReason|String|Unknow|锁定原因。 |
|MaintainEndTime|String|22:00Z|可维护截止时间。 |
|MaintainStartTime|String|18:00Z|可维护开始时间。 |
|MaxConnections|Integer|500|实例的最大并发连接数。 |
|MemoryPerNode|Integer|1664|单副本的内存大小。 |
|MemoryUnit|String|MB|内存单位。 |
|PayType|String|Prepaid|计费类型：

 -   Postpaid：按量付费
-   Prepaid：包年包月 |
|Port|String|3432|访问端口。 |
|ReadDelayTime|String|2|读延迟时间。 |
|RegionId|String|cn-hangzhou-e|地域ID。 |
|SecurityIPList|String|127.0.0.1|允许访问的IP名单。 |
|SegmentCounts|Integer|2|Segment个数。 |
|StoragePerNode|Integer|1664|单副本的存储大小。 |
|StorageType|String|HDD|存储类型。 |
|StorageUnit|String|MB|存储单位。 |
|Tags|Array of Tag| |标记键值对。 |
|Tag| | | |
|Key|String|key1|标签键。 |
|Value|String|value1|标签值。 |
|VpcId|String|vpc-xxxxxxxx|开启ClassicLink的VPC ID。 |
|ZoneId|String|cn-hangzhou|可用区ID。 |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstanceAttribute
&DBInstanceId=gp-xxxxxxx
&<公共请求参数>
```

正常返回示例

`XML`格式

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

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

