# ModifyDBInstanceMaintainTime

You can call this operation to modify the maintenance window of an AnalyticDB for PostgreSQL instance.

Typically, the maintenance window is set to the off-peak hours to minimize the impact on services. Alibaba Cloud performs instance maintenance at the maintenance window that you specify.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceMaintainTime&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifyDBInstanceMaintainTime|The operation that you want to perform. Set the value to ModifyDBInstanceMaintainTime. |
|DBInstanceId|String|Yes|gp-xxxxxxxx|The ID of the instance. |
|EndTime|String|Yes|03:00Z|The end time of the maintenance window. Example: 03:00Z. The end time must be later than the start time. |
|StartTime|String|Yes|02:00Z|The start time of the maintenance window. Example: 02:00Z. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|CA9A34C8-AC95-413B-AC6A-CEBF67429CCA|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceMaintainTime
&DBInstanceId=gp-xxxxxxxx
&EndTime=03:00Z
&StartTime=02:00Z
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ModifyDBInstanceMaintainTimeResponse>
           <RequestId>CA9A34C8-AC95-413B-AC6A-CEBF67429CCA</RequestId>
</ModifyDBInstanceMaintainTimeResponse>
```

`JSON` format

```
{
   "RequestId": "CA9A34C8-AC95-413B-AC6A-CEBF67429CCA"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

