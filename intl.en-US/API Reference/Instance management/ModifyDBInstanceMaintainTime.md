# ModifyDBInstanceMaintainTime

## Description

Modifies the maintenance window of an instance. We recommend that you set the maintenance window to an off-peak hour to minimize the impact on services. Alibaba Cloud performs routine maintenance for the instance within the maintenance window you set.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifyDBInstanceMaintainTime.|
|DBInstanceId|String|Yes|The ID of the instance.|
|StartTime|String|Yes|The start time of the maintenance window, such as 02:00Z.|
|EndTime|String|Yes|The end time of the maintenance window, such as 03:00Z. The end time must be later than the start time.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceMaintainTime
&DBInstanceId=gp-xxxxxxx
&StartTime=02:00Z
&EndTime=03:00Z
&<Common request parameters>

```

## Sample responses

**XML format**

```
<ModifyDBInstanceMaintainTimeResponse>
         <RequestId>CA9A34C8-AC95-413B-AC6A-CEBF67429CCA</RequestId>
</ModifyDBInstanceMaintainTimeResponse>
```

**JSON format**

```
{
   "RequestId": "CA9A34C8-AC95-413B-AC6A-CEBF67429CCA"
}
```

