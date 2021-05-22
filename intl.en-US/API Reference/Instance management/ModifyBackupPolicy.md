# ModifyBackupPolicy

Configures a backup policy for an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ModifyBackupPolicy&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifyBackupPolicy|The operation that you want to perform. Set the value to ModifyBackupPolicy. |
|DBInstanceId|String|Yes|gp-xxxxx|The ID of the instance. |
|PreferredBackupPeriod|String|Yes|Tuesday,Thursday,Saturday|The cycle based on which you want to perform a backup. Separate multiple values with commas \(,\). Valid values:

 -   Monday
-   Tuesday
-   Wednesday
-   Thursday
-   Friday
-   Saturday
-   Sunday |
|PreferredBackupTime|String|Yes|15:00Z-16:00Z|The backup window. Specify the backup window in the HH:mmZ-HH:mmZ format. The backup window must be in UTC. Default value: 00:00-01:00. |
|BackupRetentionPeriod|Integer|No|7|The number of days for which data backup files are retained. Default value: 7. Maximum value: 7. Valid values: 1 to 7. |
|EnableRecoveryPoint|Boolean|No|true|Specifies whether to enable automatic point-in-time backup.

 -   true
-   false

 Default value: true. |
|RecoveryPointPeriod|String|No|8|The frequency of point-in-time backup.

 -   1: per hour
-   2: per 2 hours
-   4: per 4 hours
-   8: per 8 hours

 Default value: 8. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|DA147739-AEAD-4417-9089-65E9B1D8240D|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=ModifyBackupPolicy
&DBInstanceId=gp-xxxxx
&PreferredBackupPeriod=Tuesday
&PreferredBackupTime=15:00Z-16:00Z
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>DA147739-AEAD-4417-9089-65E9B1D8240D</RequestId>
```

`JSON` format

```
{"RequestId":"DA147739-AEAD-4417-9089-65E9B1D8240D"}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

