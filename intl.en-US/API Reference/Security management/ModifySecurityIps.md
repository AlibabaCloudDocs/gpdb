# ModifySecurityIps

You can call this operation to modify the IP addresses that are allowed to access an instance.

Before you call this operation, make sure that the following requirements are met:

-   The instance is in the Running state.
-   The instance is not locked.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ModifySecurityIps&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifySecurityIps|The operation that you want to perform. Set the value to ModifySecurityIps. |
|DBInstanceId|String|Yes|gp-xxxxxxxxx|The ID of the instance. |
|SecurityIPList|String|Yes|127.0.0.1|The IP addresses listed in the whitelist. You can add up to 1,000 IP addresses to the whitelist. Separate multiple IP addresses with commas \(,\). The IP addresses must use one of the following formats:

 -   0.0.0.0/0
-   10.23.12.24. This is an IP address.
-   Classless Inter-Domain Routing \(CIDR\) blocks, such as 10.23.12.24/24, where `/24` indicates the prefix of the CIDR block is 24 bits in length. You can replace 24 with a value in the range of `[1,32]`. |
|DBInstanceIPArrayName|String|No|Default|The name of the whitelist. If you do not enter a name, IP addresses are added to the default whitelist.

 **Note:** You can create up to 50 whitelists for an instance. |
|DBInstanceIPArrayAttribute|String|No|hidden|This parameter is empty by default. You can use this parameter to distinguish whitelists that have different attributes. The whitelists that have the `hidden` attribute are not displayed in the console. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|871C698F-B43D-4D1D-ACD6-DF56B0F89978|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifySecurityIps
&DBInstanceId=gp-xxxxxxxxx
&SecurityIPList=127.0.0.1
&<common request parameters>
```

Sample success responses

`XML` format

```
<ModifySecurityIpsResponse>
           <RequestId>871C698F-B43D-4D1D-ACD6-DF56B0F89978</RequestId>
</ModifySecurityIpsResponse>
```

`JSON` format

```
{
  "RequestId":"871C698F-B43D-4D1D-ACD6-DF56B0F89978"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

