# ModifySQLCollectorPolicy

## Description

You can call this operation to enable or disable SQL collection for the instance.

## Request parameters

|Name|Type|Required|Description|
|----|----|--------|-----------|
|<Common request parameters\>|-|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifySQLCollectorPolicy.|
|DBInstanceId|String|Yes|The ID of the instance.|
|SQLCollectorStatus|String|Yes|-   Enabled: SQL collection is enabled.
-   Disabled: SQL collection is disabled. |

## Response parameters

|Name|Type|Description|
|----|----|-----------|
|<Common response parameters\>|-|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifySQLCollectorPolicy
&DBInstanceId=gp-xxxxxxx
&SQLCollectorStatus=Enable
&<Common request parameters>
```

## Sample responses

**XML format**

```
<ModifySQLCollectorPolicyResponse>
         <RequestId>4FA1F1D1-50A6-4F60-9A78-5752F2076A53</RequestId>
</ModifySQLCollectorPolicyResponse>
			
```

**JSON format**

```
{
  "RequestId":"4FA1F1D1-50A6-4F60-9A78-5752F2076A53"
}
```

