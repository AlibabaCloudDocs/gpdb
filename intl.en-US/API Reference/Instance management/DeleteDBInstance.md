# DeleteDBInstance

## Description

You can call this operation to delete an instance.

## Request parameters

|Name|Type|Required|Description|
|----|----|--------|-----------|
|<Common request parameters\>|-| Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String| Yes|The operation that you want to perform. Set the value to DeleteDBInstance.|
|DBInstanceId|String|Yes|The instance ID.|

## Response parameters

|Name |Type|Description|
|-----|----|-----------|
|<Common response parameters\>|-|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=DeleteDBInstance
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>
```

## Sample responses

**XML format**

```
<DeleteDBInstanceResponse>  
     <RequestId>65BDA532-28AF-4122-AA39-B382721EEE64</RequestId>
</DeleteDBInstanceResponse>
```

**JSON format**

```
"RequestId": " 65BDA532-28AF-4122-AA39-B382721EEE64"
```

