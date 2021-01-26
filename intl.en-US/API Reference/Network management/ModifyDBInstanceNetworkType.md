# ModifyDBInstanceNetworkType

## Description

Switches the network type of an instance between VPC and classic network.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifyDBInstanceNetworkType.|
|DBInstanceId|String|Yes|The ID of the instance.|
|InstanceNetworkType|String|Yes|The network type of the instance. Valid values:-   VPC: Virtual Private Cloud
-   Classic: classic network |
|VPCId|String|No|The ID of the VPC.|
|VSwitchId|String|No|The ID of the VSwitch. This parameter must be specified if the VPCId parameter is specified.|
|PrivateIpAddress|String|No|The private IP address of the instance. You can specify an IP address of the VPC to which the VSwitch belongs. If no IP address is specified, the system automatically assigns an IP address based on the VPC ID and VSwitch ID.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceNetworkType
&DBInstanceId=gp-xxxxxxx
&InstanceNetworkType=VPC
&<Common request parameters>
```

## Sample responses

**XML format**

```
<ModifyDBInstanceNetworkTypeResponse>
         <RequestId>2d0c35a9-f5da-44ba-852d-741e27b7eb0b</RequestId>
</ModifyDBInstanceNetworkTypeResponse>
```

**JSON format**

```
{
  "RequestId":"2d0c35a9-f5da-44ba-852d-741e27b7eb0b"
}
```

