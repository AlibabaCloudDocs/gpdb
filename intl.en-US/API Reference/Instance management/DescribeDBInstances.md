# DescribeDBInstances

You can call this operation to query the list of AnalyticDB for PostgreSQL instances.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstances&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeDBInstances|The operation that you want to perform. Set the value to DescribeDBInstances. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. |
|DBInstanceDescription|String|No|gp-xxxxxxxx|The description of the instance. |
|InstanceNetworkType|String|No|VPC|The network type of the instance. Valid values:

 -   VPC
-   Classic
-   If you do not specify this parameter, instances of all network types are returned. |
|DBInstanceIds|String|No|gp-xxxxxxxxx,gp-xxxxxxx|The ID of the instance. Separate multiple IDs with commas \(,\). |
|PageSize|Integer|No|50|The number of entries to return on each page. Valid values: 30, 50, and 100. Default value: 30. |
|PageNumber|Integer|No|1|The number of the page to return. The value must be an integer that is larger than 0. Default value: 1. |
|Tag.N.Key|String|No|key1|The key of tag N. |
|Tag.N.Value|String|No|value1|The value of tag N. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Items|Array of DBInstance| |Details about the instances. |
|DBInstance| | | |
|ConnectionMode|String|Standard|The access mode of the instance. Valid values:

 -   Standard
-   Safe |
|CreateTime|String|2019-09-08T16:00:00Z|The time when the cluster was created. |
|DBInstanceDescription|String|gp-xxxxxxxxxx|The description of the instance. |
|DBInstanceId|String|gp-xxxxxxxx|The ID of the instance. |
|DBInstanceNetType|String|Internet|The type of the network interface card \(NIC\) that is used by the instance. |
|DBInstanceStatus|String|Running|The status of the instance. For more information, see [Instance statuses](~~86944~~). |
|Engine|String|gpdb|The database engine of the instance. |
|EngineVersion|String|4.3|The version of the database engine. |
|ExpireTime|String|2019-09-08T16:00:00Z|The time when the instance is scheduled to expire. Pay-as-you-go instances do not expire. |
|InstanceDeployType|String|cluster|-   cluster: ESC
-   replicaSet: physical machine |
|InstanceNetworkType|String|VPC|The network type of the instance. Valid values:

 -   VPC
-   Classic |
|LockMode|String|Unlock|Indicates whether the instance is locked. Valid values:

 -   Unlock: The instance is not locked.
-   ManualLock: The instance is manually locked.
-   LockByExpiration: The instance is automatically locked after it expires.
-   LockByRestoration: The instance is automatically locked before a rollback.
-   LockByDiskQuota: The instance is automatically locked when the disk space is full. |
|LockReason|String|Unknow|The reason why the instance is locked. |
|PayType|String|Prepaid|The billing method of the instance. Valid values:

 -   Postpaid: pay-as-you-go
-   Prepaid: subscription |
|RegionId|String|cn-hangzhou|The region ID of the instance. |
|StorageType|String|cloud\_essd|The storage type of the instance.

 -   cloud\_essd: enhanced SSD \(ESSD\)
-   cloud\_efficiency: ultra disk |
|Tags|Array of Tag| |Details about the tags. |
|Tag| | | |
|Key|String|key1|The key of the tag. |
|Value|String|value1|The value of the tag. |
|VSwitchId|String|vsw-xxxxxxxxx|The vSwitch ID of the instance. |
|VpcId|String|vpc-xxxxxxxxxx|The VPC ID of the instance. |
|ZoneId|String|cn-hangzhou|The zone ID of the instance. |
|PageNumber|Integer|1|The number of the returned page. |
|PageRecordCount|Integer|2|The number of entries returned per page. |
|RequestId|String|BBE00C04-A3E8-4114-881D-0480A72CB92E|The ID of the request. |
|TotalRecordCount|Integer|2|The total number of entries. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstances
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<TotalRecordCount>2</TotalRecordCount>
<PageRecordCount>2</PageRecordCount>
<RequestId>BBE00C04-A3E8-4114-881D-0480A72CB92E</RequestId>
<PageNumber>1</PageNumber>
<Items>
    <DBInstance>
        <StorageType>cloud_essd</StorageType>
        <EngineVersion>4.3</EngineVersion>
        <DBInstanceStatus>Running</DBInstanceStatus>
        <ZoneId>cn-hangzhou</ZoneId>
        <DBInstanceNetType>Internet</DBInstanceNetType>
        <CreateTime>2019-09-08T16:00:00Z</CreateTime>
        <VSwitchId>vsw-xxxxxxxxx</VSwitchId>
        <PayType>Prepaid</PayType>
        <LockMode>Unlock</LockMode>
        <InstanceNetworkType>VPC</InstanceNetworkType>
        <VpcId>vpc-xxxxxxxxxx</VpcId>
        <DBInstanceId>gp-xxxxxxxx</DBInstanceId>
        <InstanceDeployType>public</InstanceDeployType>
        <ConnectionMode>Standard</ConnectionMode>
        <RegionId>cn-hangzhou</RegionId>
        <ExpireTime>2019-09-08T16:00:00Z</ExpireTime>
        <LockReason>Unknow</LockReason>
        <Engine>gpdb</Engine>
        <DBInstanceDescription>gp-xxxxxxxxxx</DBInstanceDescription>
        <Tags>
            <Tag>
                <Value>value1</Value>
                <Key>key1</Key>
            </Tag>
        </Tags>
    </DBInstance>
</Items>
```

`JSON` format

```
{"TotalRecordCount":"2","PageRecordCount":"2","RequestId":"BBE00C04-A3E8-4114-881D-0480A72CB92E","PageNumber":"1","Items":{"DBInstance":[{"StorageType":"cloud_essd","EngineVersion":"4.3","DBInstanceStatus":"Running","ZoneId":"cn-hangzhou","DBInstanceNetType":"Internet","CreateTime":"2019-09-08T16:00:00Z","VSwitchId":"vsw-xxxxxxxxx","PayType":"Prepaid","LockMode":"Unlock","InstanceNetworkType":"VPC","VpcId":"vpc-xxxxxxxxxx","DBInstanceId":"gp-xxxxxxxx","InstanceDeployType":"public","ConnectionMode":"Standard","RegionId":"cn-hangzhou","ExpireTime":"2019-09-08T16:00:00Z","LockReason":"Unknow","Engine":"gpdb","DBInstanceDescription":"gp-xxxxxxxxxx","Tags":{"Tag":[{"Value":"value1","Key":"key1"}]}}]}}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

