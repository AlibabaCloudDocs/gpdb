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
|Tag.N.Key|String|否|key1|标签键。 |
|Tag.N.Value|String|否|value1|标签值。 |
|PageSize|Integer|否|50|每页记录数，取值：30/50/100，默认值：30。 |
|PageNumber|Integer|否|1|页码，大于0且不超过Integer的最大值，默认值：1。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Items|Array| |实例详情列表。 |
|DBInstance| | | |
|ConnectionMode|String|Standard|访问模式。

 -   Standard：标准访问模式
-   Safe：高安全访问模式 |
|CreateTime|String|2019-09-08T16:00:00Z|创建时间。 |
|DBInstanceDescription|String|gp-xxxxxxxxxx|实例描述。 |
|DBInstanceId|String|gp-xxxxxxxx|实例ID。 |
|DBInstanceNetType|String|Internet|实例网卡类型。 |
|DBInstanceStatus|String|Running|实例状态，详见[实例状态表](~~86944~~)。 |
|Engine|String|gpdb|数据库类型。 |
|EngineVersion|String|4.3|数据库版本。 |
|ExpireTime|String|2019-09-08T16:00:00Z|到期时间，按量付费实例无到期时间。 |
|InstanceDeployType|String|public|集群类型。

 -   cell：独享集群
-   public：共享集群 |
|InstanceNetworkType|String|VPC|实例网络类型。

 -   VPC：专有网络类型
-   Classic：经典网络类型 |
|LockMode|String|Unlock|锁定方式。

 -   Unlock：正常
-   ManualLock：手动触发锁定
-   LockByExpiration：实例过期自动锁定
-   LockByRestoration：实例回滚前的自动锁定
-   LockByDiskQuota：实例空间满自动锁定 |
|LockReason|String|Unknow|被锁定的原因 |
|PayType|String|Prepaid|计费类型：

 -   Postpaid：按量付费
-   Prepaid：包年包月 |
|RegionId|String|cn-hangzhou|地域ID。 |
|Tags|Array| |实例标签。 |
|Tag| | | |
|Key|String|key1|标签键。 |
|Value|String|value1|标签值。 |
|VSwitchId|String|vsw-xxxxxxxxx|VSwitch ID。 |
|VpcId|String|vpc-xxxxxxxxxx|VPC ID。 |
|ZoneId|String|cn-hangzhou|可用区ID。 |
|PageNumber|Integer|1|当前页码。 |
|PageRecordCount|Integer|2|当前页记录数。 |
|RequestId|String|BBE00C04-A3E8-4114-881D-0480A72CB92E|请求ID。 |
|TotalRecordCount|Integer|2|总记录数。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstances
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<Items>
    <DBInstance>
        <LockMode>Unlock</LockMode>
        <DBInstanceNetType>1</DBInstanceNetType>
        <DBInstanceId>gp-xxxxxxx</DBInstanceId>
        <ZoneId>cn-hangzhou-e</ZoneId>
        <DBInstanceDescription>gpdb_test</DBInstanceDescription>
        <InstanceNetworkType>Classic</InstanceNetworkType>
        <VSwitchId>vsw-xxxxxxx</VSwitchId>
        <VpcId>vpc-xxxxxxx</VpcId>
        <Engine>gpdb</Engine>
        <ExpireTime>2019-06-27T16:00:00Z</ExpireTime>
        <CreateTime>2018-06-27T12:07:11Z</CreateTime>
        <RegionId>cn-hangzhou</RegionId>
        <EngineVersion>4.3</EngineVersion>
        <LockReason/>
        <DBInstanceStatus>Running</DBInstanceStatus>
        <PayType>Prepaid</PayType>
    </DBInstance>
    <DBInstance>
        <LockMode>Unlock</LockMode>
        <DBInstanceNetType>1</DBInstanceNetType>
        <DBInstanceId>gp-xxxxxxx</DBInstanceId>
        <ZoneId>cn-hangzhou-e</ZoneId>
        <DBInstanceDescription>gp-xxxxxxx</DBInstanceDescription>
        <InstanceNetworkType>Classic</InstanceNetworkType>
        <VSwitchId>vsw-xxxxxxx</VSwitchId>
        <VpcId>vpc-xxxxxxx</VpcId>
        <Engine>gpdb</Engine>
        <ExpireTime>2999-09-08T16:00:00Z</ExpireTime>
        <CreateTime>2018-06-27T11:51:46Z</CreateTime>
        <RegionId>cn-hangzhou</RegionId>
        <EngineVersion>4.3</EngineVersion>
        <LockReason/>
        <DBInstanceStatus>Running</DBInstanceStatus>
        <PayType>Postpaid</PayType>
    </DBInstance>
</Items>
<PageNumber>1</PageNumber>
<TotalRecordCount>2</TotalRecordCount>
<RequestId>BBE00C04-A3E8-4114-881D-0480A72CB92E</RequestId>
<PageRecordCount>2</PageRecordCount>
```

`JSON` 格式

```
{
    "Items":{
        "DBInstance":[
            {
                "LockMode":"Unlock",
                "DBInstanceNetType":"1",
                "DBInstanceId":"gp-xxxxxxx",
                "ZoneId":"cn-hangzhou-e",
                "DBInstanceDescription":"gpdb_test",
                "InstanceNetworkType":"Classic",
                "VSwitchId":"vsw-xxxxxxx",
                "VpcId":"vpc-xxxxxxx",
                "Engine":"gpdb",
                "ExpireTime":"2019-06-27T16:00:00Z",
                "CreateTime":"2018-06-27T12:07:11Z",
                "RegionId":"cn-hangzhou",
                "EngineVersion":"4.3",
                "LockReason":"",
                "DBInstanceStatus":"Running",
                "PayType":"Prepaid"
            },
            {
                "LockMode":"Unlock",
                "DBInstanceNetType":"1",
                "DBInstanceId":"gp-xxxxxxx",
                "ZoneId":"cn-hangzhou-e",
                "DBInstanceDescription":"gp-xxxxxxx",
                "InstanceNetworkType":"Classic",
                "VSwitchId":"vsw-xxxxxxx",
                "VpcId":"vpc-xxxxxxx",
                "Engine":"gpdb",
                "ExpireTime":"2999-09-08T16:00:00Z",
                "CreateTime":"2018-06-27T11:51:46Z",
                "RegionId":"cn-hangzhou",
                "EngineVersion":"4.3",
                "LockReason":"",
                "DBInstanceStatus":"Running",
                "PayType":"Postpaid"
            }
        ]
    },
    "PageNumber":1,
    "TotalRecordCount":2,
    "RequestId":"BBE00C04-A3E8-4114-881D-0480A72CB92E",
    "PageRecordCount":2
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

