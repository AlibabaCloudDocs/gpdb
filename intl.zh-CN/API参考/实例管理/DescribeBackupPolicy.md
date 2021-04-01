# DescribeBackupPolicy

调用DescribeBackupPolicy接口查看集群备份设置。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeBackupPolicy&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeBackupPolicy|系统规定参数。取值：DescribeBackupPolicy。 |
|DBInstanceId|String|是|gp-xxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|BackupRetentionPeriod|Integer|7|数据备份保留天数。 |
|EnableRecoveryPoint|Boolean|true|是否开启自动恢复点。

 -   true：开启；
-   false：关闭。 |
|PreferredBackupPeriod|String|Tuesday，Thursday，Saturday|数据备份周期，多个取值用英文逗号（,）隔开。取值：

 -   Monday：周一；
-   Tuesday：周二；
-   Wednesday：周三；
-   Thursday：周四；
-   Friday：周五；
-   Saturday：周六；
-   Sunday：周日。 |
|PreferredBackupTime|String|15:00Z-16:00Z|数据备份时间。格式：HH:mmZ-HH:mmZ（UTC时间）。 |
|RecoveryPointPeriod|String|8|恢复点频次。

 -   1，每小时
-   2，每两小时
-   4，每四小时
-   8，每八小时 |
|RequestId|String|1AD222E9-E606-4A42-BF6D-8A4442913CEF|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeBackupPolicy
&DBInstanceId=gp-xxxx
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<PreferredBackupPeriod>Tuesday，Thursday，Saturday</PreferredBackupPeriod>
<RequestId>1AD222E9-E606-4A42-BF6D-8A4442913CEF</RequestId>
<PreferredBackupTime>15:00Z-16:00Z</PreferredBackupTime>
<BackupRetentionPeriod>7</BackupRetentionPeriod>
<EnableRecoveryPoint>true</EnableRecoveryPoint>
<RecoveryPointPeriod>8</RecoveryPointPeriod>
```

`JSON`格式

```
{"PreferredBackupPeriod":"Tuesday，Thursday，Saturday","RequestId":"1AD222E9-E606-4A42-BF6D-8A4442913CEF","PreferredBackupTime":"15:00Z-16:00Z","BackupRetentionPeriod":"7","EnableRecoveryPoint":"true","RecoveryPointPeriod":"8"}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

