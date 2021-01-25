# AllocateInstancePublicConnection

## Description

Assigns a prefix to the public endpoint of an instance.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to AllocateInstancePublicConnection.|
|DBInstanceId|String|Yes|The ID of the instance.|
|ConnectionStringPrefix|String|Yes|The endpoint of the instance.|
|Port|String|Yes|The port of the instance. Valid values: 3200 to 3999.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```

https://gpdb.aliyuncs.com/?Action=AllocateInstancePublicConnection
&DBInstanceId=gp-xxxxxxx
&ConnectionStringPrefix=gp-xxxxxxx
&Port=3432
&<Common request parameters>
```

## Sample responses

**XML format**

```
<AllocateInstancePublicConnectionResponse>  
     <RequestId>ADD6EA90-EECB-4C12-9F26-0B6DB58710EF</RequestId>
</AllocateInstancePublicConnectionResponse>
```

**JSON format**

```
{
   "RequestId": "ADD6EA90-EECB-4C12-9F26-0B6DB58710EF"
}
```

