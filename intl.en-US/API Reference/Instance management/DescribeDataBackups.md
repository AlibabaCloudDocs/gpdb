# DescribeDataBackups

Queries the list of backup sets and points in time to which data can be restored for an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDataBackups&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeDataBackups|The operation that you want to perform. Set the value to DescribeDataBackups. |
|DBInstanceId|String|Yes|gp-xxxxx|The ID of the instance. |
|EndTime|String|Yes|2011-06-01T16:00Z|The end of the time range to query. The end time must be later than the start time. Specify the time in the yyyy-MM-ddTHH:mmZ format. The time must be in UTC. |
|StartTime|String|Yes|2011-06-01T15:00Z|The beginning of the time range to query. Specify the time in the yyyy-MM-ddTHH:mmZ format. The time must be in UTC. |
|BackupId|String|No|327329803|The ID of the backup set. If you specify the BackupId parameter, the details of the backup set are returned. |
|PageSize|Integer|No|30|The number of entries to return on each page. Valid values:

 -   30
-   50
-   100

 Default value: 30. |
|PageNumber|Integer|No|1|The number of the page to return. The value must be an integer that is larger than 0. Default value: 1. |
|DataType|String|No|DATA|The type of the backup. Valid values:

 -   DATA: full backup
-   RESTOREPOI: point-in-time backup

 If you do not specify this parameter, the records of the full backup set are returned. |
|BackupMode|String|No|Automated|The backup mode. Valid values:

 -   Automated: automatic backup
-   Manual: manual backup

 If you do not specify this parameter, the records of the backup sets in all modes are returned. |
|BackupStatus|String|No|Success|The status of the backup set. Valid values:

 -   Success: The backup is complete.
-   Failed: The backup task fails.

 If you do not specify this parameter, the records of the backup sets in all states are returned. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Items|Array of Backup| |Details about the backup sets. |
|BackupEndTime|String|2011-06-01T17:00Z|The UTC time when the backup ended. The time is in the yyyy-MM-ddTHH:mmZ format. The time is displayed in UTC. |
|BackupEndTimeLocal|String|2011-05-30 03:29:00|The local time when the backup ended. The time is in the yyyy-MM-dd HH:mm:ss format. The time is your local time. |
|BackupMode|String|Automated|The backup mode.

 Valid values for full backup:

 -   Automated: automatic backup
-   Manual: manual backup

 Valid values for point-in-time backup:

 -   Automated: point-in-time backup after full backup
-   Manual: manual point-in-time backup
-   Period: point-in-time backup that is triggered by a backup policy |
|BackupSetId|String|327329803|The ID of the backup set. |
|BackupSize|Long|2167808|The size of the backup file. Unit: bytes. |
|BackupStartTime|String|2011-06-01T17:00Z|The UTC time when the backup started. The time is in the yyyy-MM-ddTHH:mmZ format. The time is displayed in UTC. |
|BackupStartTimeLocal|String|2011-05-30 03:29:00|The local time when the backup started. The time is in the yyyy-MM-dd HH:mm:ss format. The time is your local time. |
|BackupStatus|String|Success|The status of the backup set. Valid values:

 -   Success
-   Failure |
|BaksetName|String|restorepoint\_xxx|The name of a point-in-time backup set or the full backup set. |
|ConsistentTime|Long|1576506856|-   For full backup, this parameter indicates the point in time at which the data in the data backup file is consistent.
-   For point-in-time backup, this parameter indicates that the returned point in time is a timestamp. |
|DBInstanceId|String|gp-xxxxx|The ID of the instance. |
|DataType|String|DATA|The type of the backup. Valid values:

 -   DATA: full backup
-   RESTOREPOI: point-in-time backup |
|PageNumber|Integer|10|The page number of the returned page. |
|PageSize|Integer|30|The number of backup sets on the page. |
|RequestId|String|1AD222E9-E606-4A42-BF6D-8A4442913CEF|The ID of the request. |
|TotalCount|Integer|300|The total number of entries. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=DescribeDataBackups
&DBInstanceId=gp-xxxxx
&EndTime=2011-06-01T16:00Z
&StartTime=2011-06-01T15:00Z
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

```
{"TotalCount":"300","PageSize":"30","RequestId":"1AD222E9-E606-4A42-BF6D-8A4442913CEF","PageNumber":"10","Items":[{"BackupEndTimeLocal":"2011-05-30 03:29:00","DBInstanceId":"gp-xxxxx","BackupSize":"2167808","BackupMode":"Automated","BackupEndTime":"2011-06-01T17:00Z","ConsistentTime":"1576506856","BackupStartTime":"2011-06-01T17:00Z","BaksetName":"restorepoint_xxx","DataType":"DATA","BackupStartTimeLocal":"2011-05-30 03:29:00","BackupSetId":"327329803","BackupStatus":"Success"}]}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

