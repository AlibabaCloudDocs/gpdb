# DescribeBackupPolicy

Queries the backup settings of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeBackupPolicy&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeBackupPolicy|The operation that you want to perform. Set the value to DescribeBackupPolicy. |
|DBInstanceId|String|Yes|gp-xxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|BackupRetentionPeriod|Integer|7|The number of days for which data backup files are retained. |
|EnableRecoveryPoint|Boolean|true|Indicates whether automatic point-in-time backup is enabled.

 -   true
-   false |
|PreferredBackupPeriod|String|Tuesday,Thursday,Saturday|The cycle based on which backups are performed. If more than one day of the week is specified, the days of the week are separated by commas \(,\). Valid values:

 -   Monday
-   Tuesday
-   Wednesday
-   Thursday
-   Friday
-   Saturday
-   Sunday |
|PreferredBackupTime|String|15:00Z-16:00Z|The backup time. The time is in the HH:mmZ-HH:mmZ format. The time is displayed in UTC. |
|RecoveryPointPeriod|String|8|The frequency of the point-in-time backup.

 -   1: per hour
-   2: per 2 hours
-   4: per 4 hours
-   8: per 8 hours |
|RequestId|String|1AD222E9-E606-4A42-BF6D-8A4442913CEF|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=DescribeBackupPolicy
&DBInstanceId=gp-xxxx
&<Common request parameters>
```

Sample success responses

`XML` format

```
<PreferredBackupPeriod>Tuesday, Thursday, Saturday</PreferredBackupPeriod>
<RequestId>1AD222E9-E606-4A42-BF6D-8A4442913CEF</RequestId>
<PreferredBackupTime>15:00Z-16:00Z</PreferredBackupTime>
<BackupRetentionPeriod>7</BackupRetentionPeriod>
<EnableRecoveryPoint>true</EnableRecoveryPoint>
<RecoveryPointPeriod>8</RecoveryPointPeriod>
```

`JSON` format

```
{"PreferredBackupPeriod":"Tuesday, Thursday, Saturday","RequestId":"1AD222E9-E606-4A42-BF6D-8A4442913CEF","PreferredBackupTime":"15:00Z-16:00Z","BackupRetentionPeriod":"7","EnableRecoveryPoint":"true","RecoveryPointPeriod":"8"}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

