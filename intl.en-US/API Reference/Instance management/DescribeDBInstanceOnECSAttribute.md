# DescribeDBInstanceOnECSAttribute

You can call this operation to query the details of an AnalyticDB for PostgreSQL instance that is deployed on an ECS instance.

****

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceOnECSAttribute&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeDBInstanceOnECSAttribute|The operation that you want to perform. Set the value to DescribeDBInstanceOnECSAttribute. |
|DBInstanceId|String|Yes|gp-xxxxxxxxx|The ID of the AnalyticDB for PostgreSQL instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Items|Array of DBInstanceAttribute|N/A|Details about the instance. |
|DBInstanceAttribute|N/A|N/A|N/A|
|ConnectionMode|String|Safty|-   Performance: standard connection mode
-   Safty: safe connection mode |
|ConnectionString|String|gp-xxxxxxx.gpdb.rds.aliyuncs.com|The endpoint of the instance. |
|CpuCores|Integer|8|The number of CPU cores. |
|CreationTime|String|2011-05-30T12:11:4Z|The time when the instance was created. |
|DBInstanceClass|String|gpdb.group.segsdx1|The instance type. For more information, see [Instance types](~~86942~~). |
|DBInstanceDescription|String|gp-xxxxxx|The description of the instance. The description can be up to 256 characters in length. |
|DBInstanceId|String|gp-xxxxxxxxx|The ID of the instance. |
|DBInstanceStatus|String|Running|The status of the instance. For more information, see [Instance statuses](~~86944~~). |
|EncryptionKey|String|2axxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|The ID of the encryption key. If encryption is disabled, this parameter is empty. |
|EncryptionType|String|NULL|The type of the encryption. Default value: NULL. Valid values:

-   NULL: Encryption is disabled.
-   CloudDisk: Encryption is enabled on disks and the encryption key is specified by using the EncryptionKey parameter.

Note: Disk encryption cannot be disabled after it is enabled. |
|Engine|String|gpdb|The database engine. |
|EngineVersion|String|4.3|The version of the database engine. |
|ExpireTime|String|2019-03-27T16:00:00Z|The time when the instance expires. |
|InstanceDeployType|String|public|The type of the cluster. Valid values:

-   cell: exclusive cluster
-   public: shared cluster |
|InstanceNetworkType|String|VPC|The network type of the instance. |
|LockMode|String|Unlock|The locking mode of the instance. Valid values:

-   Unlock: The instance is not locked.
-   ManualLock: The instance is manually locked.
-   LockByExpiration: The instance is automatically locked after it expires.
-   LockByRestoration: The instance is automatically locked before a rollback.
-   LockByDiskQuota: The instance is automatically locked when the disk space is full.
-   LockReadInstanceByDiskQuota: The instance is a read-only instance and is automatically locked when the disk space is full. |
|MemorySize|Integer|16|The memory capacity. Unit: GB. |
|PayType|String|Prepaid|The billing method of the instance. Valid values:

-   Postpaid: pay-as-you-go
-   Prepaid: subscription |
|Port|String|3432|The port used to connect to the instance. |
|RegionId|String|cn-hangzhou|The region ID of the instance. |
|SegNodeNum|Integer|4|The number of nodes. |
|StorageSize|Integer|200|The storage capacity. Unit: GB. |
|StorageType|String|hdd|The storage type. |
|Tags|Array of Tag|N/A|Details about the tags. |
|Tag|N/A|N/A|N/A|
|Key|String|key1|The tag key of the instance. |
|Value|String|value1|The tag value of the instance. |
|VSwitchId|String|vsw-xxxxxxxxx|The ID of the vSwitch. |
|VpcId|String|vpc-xxxxxxxxxx|The ID of the VPC. |
|ZoneId|String|cn-hangzhou-a|The zone ID. |
|RequestId|String|B67A34DF-6F96-4465-881E-57FEDD8C8735|The ID of the request. |

## Examples

Sample request

```
http(s)://[Endpoint]/? Action=DescribeDBInstanceOnECSAttribute
&DBInstanceId=gp-xxxxxxxxx
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>B67A34DF-6F96-4465-881E-57FEDD8C8735</RequestId>
<Items>
    <DBInstanceAttribute>
        <Port>3432</Port>
        <SegNodeNum>4</SegNodeNum>
        <EncryptionKey>2axxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</EncryptionKey>
        <InstanceNetworkType>VPC</InstanceNetworkType>
        <DBInstanceId>gp-xxxxxxxxx</DBInstanceId>
        <DBInstanceDescription>gp-xxxxxx</DBInstanceDescription>
        <Engine>gpdb</Engine>
        <EncryptionType>Off</EncryptionType>
        <MemorySize>16</MemorySize>
        <StorageType>hdd</StorageType>
        <EngineVersion>4.3</EngineVersion>
        <ZoneId>cn-hangzhou-a</ZoneId>
        <DBInstanceStatus>Running </DBInstanceStatus>
        <DBInstanceClass>gpdb.group.segsdx1</DBInstanceClass>
        <VSwitchId>vsw-xxxxxxxxx</VSwitchId>
        <StorageSize>200</StorageSize>
        <LockMode>Unlock</LockMode>
        <PayType>Prepaid</PayType>
        <VpcId>vpc-xxxxxxxxxx</VpcId>
        <CpuCores>8</CpuCores>
        <InstanceDeployType>public</InstanceDeployType>
        <ConnectionMode>Safty</ConnectionMode>
        <CreationTime>2011-05-30T12:11:4Z</CreationTime>
        <RegionId>cn-hangzhou</RegionId>
        <ConnectionString>gp-xxxxxxx.gpdb.rds.aliyuncs.com</ConnectionString>
        <ExpireTime>2019-03-27T16:00:00Z</ExpireTime>
    </DBInstanceAttribute>
    <DBInstanceAttribute>
        <Tags>
            <Tag>
                <Value>value1</Value>
                <Key>key1</Key>
            </Tag>
        </Tags>
    </DBInstanceAttribute>
</Items>
```

`JSON` format

```
{"RequestId":"B67A34DF-6F96-4465-881E-57FEDD8C8735","Items":{"DBInstanceAttribute":[{"Port":"3432","SegNodeNum":"4","EncryptionKey":"2axxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx","InstanceNetworkType":"VPC","DBInstanceId":"gp-xxxxxxxxx","DBInstanceDescription":"gp-xxxxxx","Engine":"gpdb","EncryptionType":"Off","MemorySize":"16","StorageType":"hdd","EngineVersion":"4.3","ZoneId":"cn-hangzhou-a","DBInstanceStatus":"Running ","DBInstanceClass":"gpdb.group.segsdx1","VSwitchId":"vsw-xxxxxxxxx","StorageSize":"200","LockMode":"Unlock","PayType":"Prepaid","VpcId":"vpc-xxxxxxxxxx","CpuCores":"8","InstanceDeployType":"public","ConnectionMode":"Safty","CreationTime":"2011-05-30T12:11:4Z","RegionId":"cn-hangzhou","ConnectionString":"gp-xxxxxxx.gpdb.rds.aliyuncs.com","ExpireTime":"2019-03-27T16:00:00Z"},{"Tags":{"Tag":[{"Value":"value1","Key":"key1"}]}}]}}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

