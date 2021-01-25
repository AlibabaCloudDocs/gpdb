# ReleaseInstancePublicConnection

## Description

Releases the public endpoint of an instance.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ReleaseInstancePublicConnection.|
|DBInstanceId|String|Yes|The ID of the instance.|
|CurrentConnectionString|String|Yes|The public endpoint of the instance.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=ReleaseInstancePublicConnection
&DBInstanceId=gp-xxxxxxx
&CurrentConnectionString=gp-xxxxxxx.gpdb.rds.aliyuncs.com
&<Common request parameters>
```

## Sample responses

**XML format**

```
<ReleaseInstancePublicConnectionResponse>  
         <RequestId>9CEF7037-4158-4A65-BEC7-2A1541F6FE74</RequestId>
</ReleaseInstancePublicConnectionResponse>
```

**JSON format**

```
{
   "RequestId": "9CEF7037-4158-4A65-BEC7-2A1541F6FE74"
}
```

