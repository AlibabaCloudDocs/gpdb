# ModifyBackupPolicy

调用ModifyBackupPolicy接口设置集群备份策略。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ModifyBackupPolicy&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyBackupPolicy|系统规定参数。取值：ModifyBackupPolicy。 |
|DBInstanceId|String|是|gp-xxxxx|实例ID。 |
|PreferredBackupPeriod|String|是|Tuesday，Thursday，Saturday|数据备份周期，多个取值用英文逗号（,）隔开。取值：

 -   Monday：周一；
-   Tuesday：周二；
-   Wednesday：周三；
-   Thursday：周四；
-   Friday：周五；
-   Saturday：周六；
-   Sunday：周日。 |
|PreferredBackupTime|String|是|15:00Z-16:00Z|数据备份时间。默认值00:00-01:00，格式：HH:mmZ-HH:mmZ（UTC时间）。 |
|BackupRetentionPeriod|Integer|否|7|数据备份保留天数。默认7天，最大值定为7天 取值范围\(1,7\)。 |
|EnableRecoveryPoint|Boolean|否|true|是否开启自动打恢复点。

 -   true：开启；
-   false：关闭。

 默认值 true。 |
|RecoveryPointPeriod|String|否|8|恢复点频次。

 -   1，每小时
-   2，每两小时
-   4，每四小时
-   8，每八小时

 默认值：8。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|DA147739-AEAD-4417-9089-65E9B1D8240D|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ModifyBackupPolicy
&DBInstanceId=gp-xxxxx
&PreferredBackupPeriod=Tuesday
&PreferredBackupTime=15:00Z-16:00Z
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<RequestId>DA147739-AEAD-4417-9089-65E9B1D8240D</RequestId>
```

`JSON`格式

```
{"RequestId":"DA147739-AEAD-4417-9089-65E9B1D8240D"}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

