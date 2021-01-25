# ModifyDBInstanceConnectionMode

## Description

Modifies the access mode of an instance. The following two modes are supported:

-   Standard mode
-   Safe mode

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifyDBInstanceConnectionMode.|
|DBInstanceId|String|Yes|The ID of the instance.|
|ConnectionMode|String|Yes|The access mode of the instance. Valid values:

 -   Standard: standard mode
-   Safe: safe mode |

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceConnectionMode
&DBInstanceId=gp-xxxxxxx
&ConnectionMode=Safe
&<Common request parameters>
			
```

## Sample responses

**XML format**

```
<BaseResponse> 
    <RequestId>8361E350-4CCC-4D28-B020-55B4DCB75D3B</RequestId>
</BaseResponse>
```

**JSON format**

```
{
    "RequestId":"8361E350-4CCC-4D28-B020-55B4DCB75D3B",
}
```

