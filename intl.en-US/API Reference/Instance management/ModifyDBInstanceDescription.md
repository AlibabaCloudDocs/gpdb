# ModifyDBInstanceDescription

## Description

You can call this API to modify the description of the instance so that you can easily identify it.

**Request parameters**

|Name|Type|Required|Description|
|----|----|--------|-----------|
|<Common request parameters\>|-|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifyDBInstanceDescription.|
|DBInstanceId|String|Yes|The ID of the instance.|
|DBInstanceDescription|Sting|Yes|The description of the instance.|

## Response parameters

|Name|Type|Description|
|----|----|-----------|
|<Common response parameters\>|-|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

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

