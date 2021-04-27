# DescribeDBInstances

调用DescribeDBInstances查询数据库实例列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstances&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeDBInstances|系统规定参数。取值：DescribeDBInstances。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |
|DBInstanceDescription|String|否|gp-xxxxxxxx|实例描述。 |
|InstanceNetworkType|String|否|VPC|实例网络类型，取值范围如下：

 -   VPC：专有网络类型
-   Classic：经典网络类型
-   不填：默认返回所有网络类型的实例 |
|DBInstanceIds|String|否|gp-xxxxxxxxx，gp-xxxxxxx|实例ID，多个实例ID之间用半角逗号分隔。 |
|PageSize|Integer|否|50|每页记录数，取值：30/50/100，默认值：30。 |
|PageNumber|Integer|否|1|页码，大于0且不超过Integer的最大值，默认值：1。 |
|Tag.N.Key|String|否|key1|标签键。 |
|Tag.N.Value|String|否|value1|标签值。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|TotalRecordCount|Integer|2|总记录数。 |
|PageRecordCount|Integer|2|当前页记录数。 |
|RequestId|String|BBE00C04-A3E8-4114-881D-0480A72CB92E|请求ID。 |
|PageNumber|Integer|1|当前页码。 |
|Items|Array of DBInstance| |实例详情列表。 |
|DBInstance| | | |
|VpcId|String|vpc-xxxxxxxxxx|VPC ID。 |
|ExpireTime|String|2019-09-08T16:00:00Z|到期时间，按量付费实例无到期时间。 |
|DBInstanceNetType|String|Internet|实例网卡类型。 |
|InstanceDeployType|String|cluster|-   cluster：ECS版本
-   replicaSet：物理机版本 |
|StorageType|String|cloud\_essd|AnalyticDB PG实例用的存储类型：

 -   cloud\_essd，ESSD云盘
-   cloud\_efficiency，高效云盘 |
|CreateTime|String|2019-09-08T16:00:00Z|创建时间。 |
|PayType|String|Prepaid|计费类型：

 -   Postpaid：按量付费
-   Prepaid：包年包月 |
|Tags|Array of Tag| |实例标签。 |
|Tag| | | |
|Key|String|key1|标签键。 |
|Value|String|value1|标签值。 |
|LockReason|String|Unknow|被锁定的原因 |
|DBInstanceStatus|String|Running|实例状态，详见[实例状态表](~~86944~~)。 |
|ConnectionMode|String|Standard|访问模式。

 -   Standard：标准访问模式
-   Safe：高安全访问模式 |
|LockMode|String|Unlock|锁定方式。

 -   Unlock：正常
-   ManualLock：手动触发锁定
-   LockByExpiration：实例过期自动锁定
-   LockByRestoration：实例回滚前的自动锁定
-   LockByDiskQuota：实例空间满自动锁定 |
|EngineVersion|String|4.3|数据库版本。 |
|RegionId|String|cn-hangzhou|地域ID。 |
|VSwitchId|String|vsw-xxxxxxxxx|VSwitch ID。 |
|InstanceNetworkType|String|VPC|实例网络类型。

 -   VPC：专有网络类型
-   Classic：经典网络类型 |
|ZoneId|String|cn-hangzhou|可用区ID。 |
|DBInstanceId|String|gp-xxxxxxxx|实例ID。 |
|Engine|String|gpdb|数据库类型。 |
|DBInstanceDescription|String|gp-xxxxxxxxxx|实例描述。 |
|SegNodeNum|String|4|节点数量。 |
|StorageSize|String|100|支持的存储大小。 |
|MasterNodeNum|Integer|1|实例MASTER数量。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstances
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML`格式

```
HTTP/1.1 200 OK
Content-Type:application/xml

<DescribeDBInstancesResponse>
    <TotalRecordCount>2</TotalRecordCount>
    <PageRecordCount>2</PageRecordCount>
    <RequestId>BBE00C04-A3E8-4114-881D-0480A72CB92E</RequestId>
    <PageNumber>1</PageNumber>
    <Items>
        <VpcId>vpc-xxxxxxxxxx</VpcId>
        <ExpireTime>2019-09-08T16:00:00Z</ExpireTime>
        <DBInstanceNetType>Internet</DBInstanceNetType>
        <InstanceDeployType>cluster</InstanceDeployType>
        <StorageType>cloud_essd</StorageType>
        <CreateTime>2019-09-08T16:00:00Z</CreateTime>
        <PayType>Prepaid</PayType>
        <Tags>
            <Key>key1</Key>
            <Value>value1</Value>
        </Tags>
        <LockReason>Unknow</LockReason>
        <DBInstanceStatus>Running</DBInstanceStatus>
        <ConnectionMode>Standard</ConnectionMode>
        <LockMode>Unlock</LockMode>
        <EngineVersion>4.3</EngineVersion>
        <RegionId>cn-hangzhou</RegionId>
        <VSwitchId>vsw-xxxxxxxxx</VSwitchId>
        <InstanceNetworkType>VPC</InstanceNetworkType>
        <ZoneId>cn-hangzhou</ZoneId>
        <DBInstanceId>gp-xxxxxxxx</DBInstanceId>
        <Engine>gpdb</Engine>
        <DBInstanceDescription>gp-xxxxxxxxxx</DBInstanceDescription>
        <SegNodeNum>4</SegNodeNum>
        <StorageSize>100</StorageSize>
        <MasterNodeNum>1</MasterNodeNum>
    </Items>
</DescribeDBInstancesResponse>
```

`JSON`格式

```
HTTP/1.1 200 OK
Content-Type:application/json

{
  "TotalRecordCount" : 2,
  "PageRecordCount" : 2,
  "RequestId" : "BBE00C04-A3E8-4114-881D-0480A72CB92E",
  "PageNumber" : 1,
  "Items" : [ {
    "VpcId" : "vpc-xxxxxxxxxx",
    "ExpireTime" : "2019-09-08T16:00:00Z",
    "DBInstanceNetType" : "Internet",
    "InstanceDeployType" : "cluster",
    "StorageType" : "cloud_essd",
    "CreateTime" : "2019-09-08T16:00:00Z",
    "PayType" : "Prepaid",
    "Tags" : [ {
      "Key" : "key1",
      "Value" : "value1"
    } ],
    "LockReason" : "Unknow",
    "DBInstanceStatus" : "Running",
    "ConnectionMode" : "Standard",
    "LockMode" : "Unlock",
    "EngineVersion" : "4.3",
    "RegionId" : "cn-hangzhou",
    "VSwitchId" : "vsw-xxxxxxxxx",
    "InstanceNetworkType" : "VPC",
    "ZoneId" : "cn-hangzhou",
    "DBInstanceId" : "gp-xxxxxxxx",
    "Engine" : "gpdb",
    "DBInstanceDescription" : "gp-xxxxxxxxxx",
    "SegNodeNum" : "4",
    "StorageSize" : "100",
    "MasterNodeNum" : 1
  } ]
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

