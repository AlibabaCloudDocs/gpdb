# ModifyDBInstanceConnectionString

## Description

Modifies the internal or public endpoint and port of an instance.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifyDBInstanceConnectionString.|
|DBInstanceId|String|Yes|The ID of the instance.|
|CurrentConnectionString|String|Yes|The original endpoint of the instance.|
|ConnectionStringPrefix|String|Yes|The new endpoint of the instance.|
|Port|String|No|The new port of the instance.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceConnectionString
&DBInstanceId=gp-xxxxxxx
&CurrentConnectionString=gp-xxxxxxx.gpdb.rds.aliyuncs.com
&ConnectionStringPrefix=gp-xxxxxxx
&<Common request parameters>
```

## Sample responses

**XML format**

```
<ModifyDBInstanceConnectionStringResponse>
         <RequestId>29B0BF34-D069-4495-92C7-FA6D94528A9E</RequestId>
</ModifyDBInstanceConnectionStringResponse>
```

**JSON format**

```
{
      "RequestId":"29B0BF34-D069-4495-92C7-FA6D94528A9E"
  }
```

