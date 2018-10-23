# ModifySQLCollectorPolicy {#concept_wmq_fhm_q2b .concept}

## Description { .section}

You can call this operation to enable or disable SQL collection for the instance.

## Request parameters { .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|<Common request parameters\>|-|Yes|For more information, see [Common parameters](reseller.en-US/API Reference/Common parameters.md#).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifySQLCollectorPolicy.|
|DBInstanceId|String|Yes|The ID of the instance.|
|SQLCollectorStatus|String| Yes| -   Enabled: SQL collection is enabled.
-   Disabled: SQL collection is disabled.

 |

## Response parameters { .section}

|Name |Type|Description|
|-----|----|-----------|
|<Common response parameters\>|-|For more information, see [Common response parameters](reseller.en-US/API Reference/Common parameters.md#section_apd_1rv_3bb).|

## Sample requests { .section}

```
https://gpdb.aliyuncs.com/?Action=ModifySQLCollectorPolicy
&DBInstanceId=gp-xxxxxxx
&SQLCollectorStatus=Enable
&<Common request parameters>
```

## Sample responses { .section}

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

