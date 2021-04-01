# DescribeDataBackups

调用DescribeDataBackups接口查看集群数据备份列表和可恢复点。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDataBackups&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeDataBackups|系统规定参数。取值：DescribeDataBackups。 |
|DBInstanceId|String|是|gp-xxxxx|实例ID。 |
|EndTime|String|是|2011-06-01T16:00Z|查询结束时间，需要大于查询开始时间。格式： yyyy-MM-ddTHH:mmZ（UTC时间）。 |
|StartTime|String|是|2011-06-01T15:00Z|查询开始时间。格式： yyyy-MM-ddTHH:mmZ（UTC时间）。 |
|BackupId|String|否|327329803|备份集ID。如果带上BackupId，则是查询该备份详情。 |
|PageSize|Integer|否|30|每页记录数。取值：

 -   30；
-   50；
-   100；

 默认值：30。 |
|PageNumber|Integer|否|1|页码，取值：大于0且不超过Integer的最大值。默认值：1。 |
|DataType|String|否|DATA|备份类型。取值：

 -   DATA：全量备份。
-   RESTOREPOI：可恢复点。

 如果不传，则默认返回全量备份集的记录。 |
|BackupMode|String|否|Automated|备份模式。取值：

 -   Automated：系统自动备份。
-   Manual：手动备份。

 如果不传，则返回所有。 |
|BackupStatus|String|否|Success|备份集状态。取值：

 -   Success：已完成备份。
-   Failed：备份失败。

 如果不传，则返回所有。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Items|Array of Backup| |备份集列表。 |
|BackupEndTime|String|2011-06-01T17:00Z|备份结束时间。格式：yyyy-MM-ddTHH:mmZ（UTC时间）。 |
|BackupEndTimeLocal|String|2011-05-30 03:29:00|本次备份结束时间。格式：yyyy-MM-dd HH:mm:ss （当地时间）。 |
|BackupMode|String|Automated|备份模式。

 对全量备份取值：

 -   Automated：系统自动备份。
-   Manual：手动备份。

 对恢复点取值：

 -   Automated：全量备份后的恢复点。
-   Manual：用户手动触发的恢复点。
-   Period：因为备份策略，定时触发的恢复点。 |
|BackupSetId|String|327329803|备份集ID。 |
|BackupSize|Long|2167808|备份文件大小，单位：Byte。 |
|BackupStartTime|String|2011-06-01T17:00Z|备份开始时间。格式：yyyy-MM-ddTHH:mmZ（UTC时间）。 |
|BackupStartTimeLocal|String|2011-05-30 03:29:00|本次备份开始时间。格式：yyyy-MM-dd HH:mm:ss （当地时间）。 |
|BackupStatus|String|Success|备份集状态。取值：

 -   Success：成功。
-   Failure：失败。 |
|BaksetName|String|restorepoint\_xxx|恢复点名称或全量备份集名称。 |
|ConsistentTime|Long|1576506856|-   对全量备份：表示一致性时间点。
-   对可恢复点：表示可恢复点时间戳返回值为时间戳。 |
|DBInstanceId|String|gp-xxxxx|实例ID。 |
|DataType|String|DATA|备份类型。取值：

 -   DATA: 全量备份。
-   RESTOREPOI：可恢复点。 |
|PageNumber|Integer|10|页码。 |
|PageSize|Integer|30|本页备份集个数。 |
|RequestId|String|1AD222E9-E606-4A42-BF6D-8A4442913CEF|请求ID。 |
|TotalCount|Integer|300|总记录数。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeDataBackups
&DBInstanceId=gp-xxxxx
&EndTime=2011-06-01T16:00Z
&StartTime=2011-06-01T15:00Z
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<TotalCount>300</TotalCount>
<PageSize>30</PageSize>
<RequestId>1AD222E9-E606-4A42-BF6D-8A4442913CEF</RequestId>
<PageNumber>10</PageNumber>
<Items>
    <BackupEndTimeLocal>2011-05-30 03:29:00</BackupEndTimeLocal>
    <DBInstanceId>gp-xxxxx</DBInstanceId>
    <BackupSize>2167808</BackupSize>
    <BackupMode>Automated</BackupMode>
    <BackupEndTime>2011-06-01T17:00Z</BackupEndTime>
    <ConsistentTime>1576506856</ConsistentTime>
    <BackupStartTime>2011-06-01T17:00Z</BackupStartTime>
    <BaksetName>restorepoint_xxx</BaksetName>
    <DataType>DATA</DataType>
    <BackupStartTimeLocal>2011-05-30 03:29:00</BackupStartTimeLocal>
    <BackupSetId>327329803</BackupSetId>
    <BackupStatus>Success</BackupStatus>
</Items>
```

`JSON`格式

```
{"TotalCount":"300","PageSize":"30","RequestId":"1AD222E9-E606-4A42-BF6D-8A4442913CEF","PageNumber":"10","Items":[{"BackupEndTimeLocal":"2011-05-30 03:29:00","DBInstanceId":"gp-xxxxx","BackupSize":"2167808","BackupMode":"Automated","BackupEndTime":"2011-06-01T17:00Z","ConsistentTime":"1576506856","BackupStartTime":"2011-06-01T17:00Z","BaksetName":"restorepoint_xxx","DataType":"DATA","BackupStartTimeLocal":"2011-05-30 03:29:00","BackupSetId":"327329803","BackupStatus":"Success"}]}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

