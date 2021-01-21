# RestartDBInstance

## Description

Restarts an instance.

## Request parameters

|**Parameter**|**Type**|**Required**|**Description**|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to RestartDBInstance.|
|DBInstanceId|String|Yes|The ID of the instance.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=RestartDBInstance
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>
```

## Sample responses

**XML format**

```
<RestartDBInstance>
         <RequestId>A7356493-7141-4393-8951-CDA8AB5D67EC</RequestId>
</RestartDBInstance>
```

**JSON format**

```
{
  "RequestId":"A7356493-7141-4393-8951-CDA8AB5D67EC"
}
```

