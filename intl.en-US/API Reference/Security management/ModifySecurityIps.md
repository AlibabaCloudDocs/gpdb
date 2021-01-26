# ModifySecurityIps

## Description

Modifies an IP address whitelist of an AnalyticDB for PostgreSQL instance. Before you call this operation, make sure that the following requirements are met:

-   The instance is in the Running state.
-   The instance is not locked.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifySecurityIps.|
|DBInstanceId|String|Yes|The ID of the instance.|
|SecurityIPList|String|Yes|The IP address whitelist. Each IP address whitelist can contain up to 1,000 IP addresses that are separated with commas \(,\). Format examples: -   0.0.0.0/0
-   10.23.12.24 \(This is an IP address.\)
-   10.23.12.24/24 \(This is a CIDR block in which /24 indicates that the prefix of the CIDR block is 24 bits in length. You can replace 24 with a value that ranges from 1 to 32. CIDR is short for Classless Inter-Domain Routing.\) |
|DBInstanceIPArrayName|String|No|The name of the IP address whitelist. If you do not specify this parameter, this operation takes effect on the IP address whitelist labeled default. **Note:** An instance can contain up to 50 IP address whitelists. |
|DBInstanceIPArrayAttribute|String|No|This parameter is empty by default. You can use this parameter to specify an attribute for an IP address whitelist. If you specify the `hidden` attribute for an IP address whitelist, that IP address whitelist does not appear in the AnalyticDB for PostgreSQL console.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifySecurityIps
&DBInstanceId=gp-xxxxxxx
&SecurityIPList=192.168.0.1
&<Common request parameters>
```

## Sample responses

**XML format**

```
<ModifySecurityIpsResponse>
         <RequestId>871C698F-B43D-4D1D-ACD6-DF56B0F89978</RequestId>
</ModifySecurityIpsResponse>
```

**JSON format**

```
{
  "RequestId":"871C698F-B43D-4D1D-ACD6-DF56B0F89978"
}
```

