# ModifySecurityIps {#concept_lb2_vgm_q2b .concept}

## Description { .section}

You can call this operation to modify the whitelist. The whitelist includes the IP addresses that are allowed to access the database instance. An instance must meet the following conditions when you call this API:

-   The instance is running.
-   The instance has not been locked.

## Request parameters { .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|<Common request parameters\>|-|Yes|For more information, see [Common parameters](reseller.en-US/API Reference/Common parameters.md#).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifySecurityIps.|
|DBInstanceId|String|Yes|The instance name.|
|SecurityIPList|String| Yes|You can add up to 1000 IP addresses under a whitelist. Multiple IP addresses are separated by commas. The format is as follows:-   0.0.0.0/0
-   10.23.12.24 \(IP\)
-   10.23.12.24/24 represents the IPv4 address 10.23.12.24. Its subnet mask is 255.255.255.0. /24 represents the length of a routing prefix. The length ranges from 1 to 32. The IP address is expressed as a Classless Inter-Domain Routing \(CIDR\) notation.

|
|DBInstanceIPArrayName| String| No|The name of a whitelist. If you do not enter the name, IP addresses are added under the default whitelist.**Note:** You can create up to 50 whitelists for an instance.

|
|DBInstanceIPArrayAttribute|String|No|The parameter is empty by default. You can use this parameter to specify an attribute for a whitelist. A whitelist with the `hidden` attribute does not appear in the console.|

## Response parameters { .section}

|Name |Type|Description|
|-----|----|-----------|
|<Common response parameters\>|-|For more information, see [Common response parameters](reseller.en-US/API Reference/Common parameters.md#section_apd_1rv_3bb).|

## Sample requests { .section}

```
https://gpdb.aliyuncs.com/?Action=ModifySecurityIps
&DBInstanceId=gp-xxxxxxx
&SecurityIPList=192.168.0.1
&<Common request parameters>
```

## Sample responses { .section}

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

