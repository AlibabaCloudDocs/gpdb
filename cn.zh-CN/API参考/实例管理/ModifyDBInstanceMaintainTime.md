# ModifyDBInstanceMaintainTime

调用ModifyDBInstanceMaintainTime修改实例的例行可维护时间。

一般设置为业务的低峰时段，将维护对业务的影响降到最低。阿里云会在您设置的可维护时段内进行实例维护。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceMaintainTime&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyDBInstanceMaintainTime|系统规定参数。取值：ModifyDBInstanceMaintainTime。 |
|DBInstanceId|String|是|gp-xxxxxxxx|实例ID。 |
|EndTime|String|是|03:00Z|可运维的结束时间。例如：03:00Z（开始时间应晚于结束时间）。 |
|StartTime|String|是|02:00Z|可运维的开始时间。例如：02:00Z。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|CA9A34C8-AC95-413B-AC6A-CEBF67429CCA|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceMaintainTime
&DBInstanceId=gp-xxxxxxxx
&EndTime=03:00Z
&StartTime=02:00Z
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<ModifyDBInstanceMaintainTimeResponse>
           <RequestId>CA9A34C8-AC95-413B-AC6A-CEBF67429CCA</RequestId>
</ModifyDBInstanceMaintainTimeResponse>
```

`JSON`格式

```
{
   "RequestId": "CA9A34C8-AC95-413B-AC6A-CEBF67429CCA"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

