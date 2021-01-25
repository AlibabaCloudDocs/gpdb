# ModifyDBInstanceDescription

## Description

Modifies the description of an instance.

**Request parameters**

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifyDBInstanceDescription.|
|DBInstanceId|String|Yes|The ID of the instance.|
|DBInstanceDescription|String|Yes|The description of the instance.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceDescription
&DBInstanceId=gp-xxxxxxx
&DBInstanceDescription=testInstanceDescribe
&<Common request parameters>

```

## Sample responses

**XML format**

```
<ModifyDBInstanceDescriptionResponse>
         <RequestId>107BE202-D1A2-479E-98E0-A892B472562F</RequestId>
</ModifyDBInstanceDescriptionResponse>
```

**JSON format**

```
{
    "RequestId": "107BE202-D1A2-479E-98E0-A892B472562F"
}
```

