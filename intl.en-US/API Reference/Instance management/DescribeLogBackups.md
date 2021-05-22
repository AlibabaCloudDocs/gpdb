# DescribeLogBackups

Queries the list of log backup sets.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeLogBackups&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeLogBackups|The operation that you want to perform. Set the value to DescribeLogBackups. |
|DBInstanceId|String|Yes|gp-xxxxx|The ID of the instance. |
|EndTime|String|Yes|2011-06-15T16:00Z|The end of the time range to query. The end time must be later than the start time. Specify the time in the yyyy-MM-ddTHH:mmZ format. The time must be in UTC. |
|StartTime|String|Yes|2011-06-15T15:00Z|The beginning of the time range to query. Specify the time in the yyyy-MM-ddTHH:mmZ format. The time must be in UTC. |
|PageSize|Integer|No|30|The number of entries to return on each page. Valid values:

 -   30
-   50
-   100

 Default value: 30. |
|PageNumber|Integer|No|1|The number of the page to return. The value must be an integer that is greater than 0. Default value: 1. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Items|Array of Backup| |Details about the list of backup sets. |
|BackupId|String|327329803|The ID of the backup set. |
|DBInstanceId|String|gp-xxxxx|The ID of the instance. |
|LogFileName|String|000000010000000300000006|The name of the log backup set that is stored in Object Storage Service \(OSS\). |
|LogFileSize|Long|2167808|The size of the log backup set. Unit: bytes. |
|LogTime|String|2021-02-24T10:55:47Z|The timestamp of the log. |
|SegmentName|String|segment-x|The name of the node. |
|PageNumber|Integer|1|The page number of the returned page. |
|PageSize|Integer|30|The number of backup sets on the page. |
|RequestId|String|1AD222E9-E606-4A42-BF6D-8A4442913CEF|The ID of the request. |
|TotalCount|Integer|30|The total number of entries. |
|TotalLogSize|Long|1073741028|The total size of logs in the time range. Unit: bytes. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=DescribeLogBackups
&DBInstanceId=gp-xxxxx
&EndTime=2011-06-15T16:00Z
&StartTime=2011-06-15T15:00Z
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

```
{"TotalCount":"30","PageSize":"30","RequestId":"1AD222E9-E606-4A42-BF6D-8A4442913CEF","PageNumber":"1","Items":[{"SegmentName":"segment-x","DBInstanceId":"gp-xxxxx","LogTime":"2021-02-24T10:55:47Z","LogFileSize":"2167808","BackupId":"327329803","LogFileName":"000000010000000300000006"}],"TotalLogSize":"1073741028"}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

