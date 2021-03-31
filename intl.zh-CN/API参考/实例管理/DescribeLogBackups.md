# DescribeLogBackups

调用DescribeLogBackups接口查看日志备份列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeLogBackups&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeLogBackups|系统规定参数。取值：DescribeLogBackups。 |
|DBInstanceId|String|是|gp-xxxxx|实例ID。 |
|EndTime|String|是|2011-06-15T16:00Z|查询结束时间，需要大于查询开始时间。格式：yyyy-MM-ddTHH:mmZ（UTC时间）。 |
|StartTime|String|是|2011-06-15T15:00Z|查询开始时间。格式： yyyy-MM-ddTHH:mmZ（UTC时间）。 |
|PageSize|Integer|否|30|每页记录数，取值：

 -   30；
-   50；
-   100。

 默认值：30。 |
|PageNumber|Integer|否|1|页码，取值：大于0且不超过Integer的最大值。默认值：1。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Items|Array of Backup| |备份集列表。 |
|BackupId|String|327329803|备份集ID。 |
|DBInstanceId|String|gp-xxxxx|实例ID。 |
|LogFileName|String|000000010000000300000006|日志文件名（OSS路径）。 |
|LogFileSize|Long|2167808|备份日志文件大小，单位：Byte。 |
|LogTime|String|2021-02-24T10:55:47Z|日志时间戳。 |
|SegmentName|String|segment-x|节点名称。 |
|PageNumber|Integer|1|页码。 |
|PageSize|Integer|30|本页备份集个数。 |
|RequestId|String|1AD222E9-E606-4A42-BF6D-8A4442913CEF|请求ID。 |
|TotalCount|Integer|30|总记录数。 |
|TotalLogSize|Long|1073741028|符合时间范围内的总日志大小，单位为Byte。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeLogBackups
&DBInstanceId=gp-xxxxx
&EndTime=2011-06-15T16:00Z
&StartTime=2011-06-15T15:00Z
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<TotalCount>30</TotalCount>
<PageSize>30</PageSize>
<RequestId>1AD222E9-E606-4A42-BF6D-8A4442913CEF</RequestId>
<PageNumber>1</PageNumber>
<Items>
    <SegmentName>segment-x</SegmentName>
    <DBInstanceId>gp-xxxxx</DBInstanceId>
    <LogTime>2021-02-24T10:55:47Z</LogTime>
    <LogFileSize>2167808</LogFileSize>
    <BackupId>327329803</BackupId>
    <LogFileName>000000010000000300000006</LogFileName>
</Items>
<TotalLogSize>1073741028</TotalLogSize>
```

`JSON`格式

```
{"TotalCount":"30","PageSize":"30","RequestId":"1AD222E9-E606-4A42-BF6D-8A4442913CEF","PageNumber":"1","Items":[{"SegmentName":"segment-x","DBInstanceId":"gp-xxxxx","LogTime":"2021-02-24T10:55:47Z","LogFileSize":"2167808","BackupId":"327329803","LogFileName":"000000010000000300000006"}],"TotalLogSize":"1073741028"}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

