# DescribeDBInstanceNetInfo

## Description

Queries the connection information of an instance.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to DescribeDBInstanceNetInfo.|
|DBInstanceId|String|Yes|The ID of the instance.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>| |For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|
|DBInstanceNetInfos|List<DBInstanceNetInfo\>|The connection information of the instance.|
|InstanceNetworkType|String|The network type of the instance. Valid values: -   Classic: classic network
-   VPC: Virtual Private Cloud |

|Parameter|Type|Description|
|---------|----|-----------|
|ConnectionString|String|The DNS connection string.|
|IPAddress|String|The IP address of the instance.|
|IPType|String|The IP address type of the instance. -   Valid values for instances in a classic network: Inner and Public
-   Valid values for instances in a VPC: Private and Public |
|Port|String|The port of the instance.|
|VPCId|String|The ID of the VPC.|
|VSwitchId|String|The ID of the VSwitch. If there are multiple IDs, they are separated with commas \(,\).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstanceNetInfo
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>
```

## Sample responses

**XML format**

```
<DescribeDBInstanceNetInfoResponse>
    <instanceNetworkType>Classic</instanceNetworkType>
    <DBInstanceNetInfos>
        <DBInstanceNetInfo>
            <DBInstanceNetType>1</DBInstanceNetType>
            <connectionString>gp-xxxxxxx.gpdb.rds.aliyuncs.com</connectionString>
            <ipAddress>127.0.0.1</ipAddress>
            <port>3432</port>
            <userVisible>1</userVisible>
        </DBInstanceNetInfo>
    </DBInstanceNetInfos>
    <RequestId>7565770E-7C45-462D-BA4A-8A5396F2CAD1</RequestId>
</DescribeDBInstanceNetInfoResponse>
```

**JSON format**

```
{
    "instanceNetworkType":"Classic",
    "DBInstanceNetInfos": {
      "DBInstanceNetInfo": [
        {
           "DBInstanceNetType":"1",
            "connectionString":"gp-xxxxxxx.gpdb.rds.aliyuncs.com",
            "ipAddress":"127.0.0.1",
            "port":"3432",
            "userVisible":1
        }
      ]
    }, 
    "RequestId": "7565770E-7C45-462D-BA4A-8A5396F2CAD1"
  }
```

