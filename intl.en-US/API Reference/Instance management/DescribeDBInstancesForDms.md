# DescribeDBInstancesForDms

You can call this operation to query instance information based on the ID of an Alibaba Cloud account.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstancesForDms&type=RPC&version=2019-06-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeDBInstancesForDms|The operation that you want to perform. Set the value to DescribeDBInstancesForDms. |
|AliUid|Long|Yes|1123456788|The ID of the Alibaba Cloud account. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|"200"|The internal status code. |
|Count|Long|1|The number of instances. |
|HttpStatusCode|Integer|200|The HTTP status code. |
|Instances|Array of Instances| |Details about the instances. |
|AliUid|String|1234567890|The ID of the Alibaba Cloud account. |
|Bid|String|1234|The ID of the account group. |
|ConnectionString|String|gp-xxxxxxxxxxxxxxxx-master.gpdbmaster.singapore.rds.aliyuncs.com|The endpoint of the instance. |
|DbInstanceName|String|gp-xxxxxxxxxxxxxxxx|The ID of the instance. |
|DbType|String|gpdb|The database engine of the instance. |
|Description|String|""|The description of the instance. |
|InstanceNetworkType|String|vpc|The network type of the instance. |
|Port|String|5432|The port number that is used to connect to the database. |
|Region|String|ap-southeast-1|The region ID of the instance. |
|VSwitchId|String|vsw-xxxxxxxxxxxxxxxxx|The vSwitch ID of the instance. |
|Version|String|6.0|The version of the instance. |
|VpcCloudInstanceId|String|gp-xxxxxxxxxxxxxxxxx-master-202103122033|The ID of the VPC-type instance. |
|VpcId|String|vpc-xxxxxxxxxxxxxxxxx|The VPC ID of the instance. |
|VpcIp|String|172.16.0.140|The IP address of the VPC. |
|Message|String|""|The returned error message. |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeDBInstancesForDms
&AliUid=1123456788
&<Common request parameters>
```

Sample success responses

`XML` format

```
<Instances>
    <VpcIp>172.16.0.140</VpcIp>
    <VSwitchId>vsw-xxxxxxxxxxxxxxxxx</VSwitchId>
    <Port>5432</Port>
    <DbType>gpdb</DbType>
    <InstanceNetworkType>vpc</InstanceNetworkType>
    <VpcId>vpc-xxxxxxxxxxxxxxxxx</VpcId>
    <Version>6.0</Version>
    <VpcCloudInstanceId>gp-xxxxxxxxxxxxxxxxx-master-202103122033</VpcCloudInstanceId>
    <Region>ap-southeast-1</Region>
    <ConnectionString>gp-xxxxxxxxxxxxxxxx-master.gpdbmaster.singapore.rds.aliyuncs.com</ConnectionString>
    <Bid>12345</Bid>
    <DbInstanceName>gp-xxxxxxxxxxxx</DbInstanceName>
    <AliUid>1234567890</AliUid>
</Instances>
<Message/>
<RequestId>B4CAF581-2AC7-41AD-8940-D56DF7AADF5B</RequestId>
<HttpStatusCode>200</HttpStatusCode>
<Count>1</Count>
<Code>200</Code>
<Success>true</Success>
```

`JSON` format

```
{
  "Instances": [
    {
    "VpcIp": "172.16.0.140",
    "VSwitchId": "vsw-xxxxxxxxxxxxxxxxx",
    "Port": "5432",
    "DbType": "gpdb",
    "InstanceNetworkType": "vpc",
    "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
    "Version": "6.0",
    "VpcCloudInstanceId": "gp-xxxxxxxxxxxxxxxxx-master-202103122033",
    "Region": "ap-southeast-1",
    "ConnectionString": "gp-xxxxxxxxxxxxxxxx-master.gpdbmaster.singapore.rds.aliyuncs.com",
    "Bid": "12345",
    "DbInstanceName": "gp-xxxxxxxxxxxx",
    "AliUid": "1234567890"
    }
    ],
    "Message": "",
    "RequestId": "B4CAF581-2AC7-41AD-8940-D56DF7AADF5B",
    "HttpStatusCode": 200,
    "Count": 1,
    "Code": "200",
    "Success": true
  }
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

