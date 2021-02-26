# ModifyDBInstanceNetworkType

You can call this operation to switch the network type of an AnalyticDB for PostgreSQL instance between VPC and classic network.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceNetworkType&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|DBInstanceId|String|Yes|gp-xxxxxxxx|The ID of the instance. |
|InstanceNetworkType|String|Yes|VPC|The network type of the instance. Valid values:

 -   VPC
-   Classic |
|VPCId|String|No|vpc-xxxxxxxx|The VPC ID of the instance. |
|VSwitchId|String|No|vsw-xxxxxxxx|The vSwitch ID of the instance. This parameter is required if the VPCId parameter is specified. |
|PrivateIpAddress|String|No|192.168.0.10|The private IP address of the instance. You can specify an IP address of the VPC to which the vSwitch belongs. If no IP address is specified, the system assigns an IP address based on the VPC ID and vSwitch ID. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|2d0c35a9-f5da-44ba-852d-741e27b7eb0b|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceNetworkType
&DBInstanceId=gp-xxxxxxxx
&InstanceNetworkType=VPC
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ModifyDBInstanceNetworkTypeResponse>
           <RequestId>2d0c35a9-f5da-44ba-852d-741e27b7eb0b</RequestId>
</ModifyDBInstanceNetworkTypeResponse>
```

`JSON` format

```
{
  "RequestId":"2d0c35a9-f5da-44ba-852d-741e27b7eb0b"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

