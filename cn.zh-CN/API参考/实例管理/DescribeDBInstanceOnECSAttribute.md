# DescribeDBInstanceOnECSAttribute

调用DescribeDBInstanceOnECSAttribute接口查询ECS形态的AnalyticDB for PostgreSQL实例详情。

****

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceOnECSAttribute&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeDBInstanceOnECSAttribute|系统规定参数。取值：DescribeDBInstanceOnECSAttribute。 |
|DBInstanceId|String|是|gp-xxxxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Items|Array of DBInstanceAttribute| |实例详情列表。 |
|DBInstanceAttribute| | | |
|ConnectionMode|String|Safty|-   Performance：标准访问模式。
-   Safty：高安全访问模式。 |
|ConnectionString|String|gp-xxxxxxx.gpdb.rds.aliyuncs.com|连接地址。 |
|CpuCores|Integer|8|CPU核数。 |
|CreationTime|String|2011-05-30T12:11:4Z|创建时间。 |
|DBInstanceClass|String|gpdb.group.segsdx1|实例规格，详见[实例规格表](~~86942~~)。 |
|DBInstanceDescription|String|gp-xxxxxx|AnalyticDB for PostgreSQL实例描述，长度为256字符。 |
|DBInstanceId|String|gp-xxxxxxxxx|实例ID。 |
|DBInstanceStatus|String|Running|实例状态，详见[实例状态表](~~86944~~) |
|EncryptionKey|String|2axxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|加密的密钥ID，如果未启用加密则为空。 |
|EncryptionType|String|NULL|加密类型，可选项为：

 -   NULL：不启用加密（默认值）
-   CloudDisk：云盘加密并通过EncryptionKey参数指定密钥

 注意：当前云盘加密开启后无法关闭。 |
|Engine|String|gpdb|数据库。 |
|EngineVersion|String|4.3|数据库版本。 |
|ExpireTime|String|2019-03-27T16:00:00Z|到期时间。 |
|InstanceDeployType|String|public|集群类型。

 -   cell：独享集群
-   public：共享集群 |
|InstanceNetworkType|String|VPC|实例的网络类型。 |
|LockMode|String|Unlock|实例锁定模式：

 -   Unlock：正常
-   ManualLock：手动触发锁定
-   LockByExpiration：实例过期自动锁定
-   LockByRestoration：实例回滚前的自动锁定
-   LockByDiskQuota：实例空间满自动锁定
-   LockReadInstanceByDiskQuota：只读实例空间满自动锁定 |
|MemorySize|Integer|16|内存容量（GB）。 |
|PayType|String|Prepaid|计费类型：

 -   Postpaid：按量付费
-   Prepaid：包年包月 |
|Port|String|3432|端口。 |
|RegionId|String|cn-hangzhou|地域ID。 |
|SegNodeNum|Integer|4|节点数量。 |
|StorageSize|Integer|200|存储空间大小，单位GB。 |
|StorageType|String|hdd|存储类型。 |
|Tags|Array of Tag| |实例标签。 |
|Tag| | | |
|Key|String|key1|标签键。 |
|Value|String|value1|标签值。 |
|VSwitchId|String|vsw-xxxxxxxxx|VSwitch ID。 |
|VpcId|String|vpc-xxxxxxxxxx|VPC的ID。 |
|ZoneId|String|cn-hangzhou-a|可用区ID。 |
|RequestId|String|B67A34DF-6F96-4465-881E-57FEDD8C8735|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeDBInstanceOnECSAttribute
&DBInstanceId=gp-xxxxxxxxx
&<公共请求参数>
```

正常返回示例

`XML` 格式

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

`JSON` 格式

```
{"RequestId":"B67A34DF-6F96-4465-881E-57FEDD8C8735","Items":{"DBInstanceAttribute":[{"Port":"3432","SegNodeNum":"4","EncryptionKey":"2axxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx","InstanceNetworkType":"VPC","DBInstanceId":"gp-xxxxxxxxx","DBInstanceDescription":"gp-xxxxxx","Engine":"gpdb","EncryptionType":"Off","MemorySize":"16","StorageType":"hdd","EngineVersion":"4.3","ZoneId":"cn-hangzhou-a","DBInstanceStatus":"Running ","DBInstanceClass":"gpdb.group.segsdx1","VSwitchId":"vsw-xxxxxxxxx","StorageSize":"200","LockMode":"Unlock","PayType":"Prepaid","VpcId":"vpc-xxxxxxxxxx","CpuCores":"8","InstanceDeployType":"public","ConnectionMode":"Safty","CreationTime":"2011-05-30T12:11:4Z","RegionId":"cn-hangzhou","ConnectionString":"gp-xxxxxxx.gpdb.rds.aliyuncs.com","ExpireTime":"2019-03-27T16:00:00Z"},{"Tags":{"Tag":[{"Value":"value1","Key":"key1"}]}}]}}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

