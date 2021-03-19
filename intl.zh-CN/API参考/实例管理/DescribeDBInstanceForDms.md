# DescribeDBInstanceForDms

调取DescribeDBInstanceForDms接口查询实例详细信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceForDms&type=RPC&version=2019-06-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeDBInstanceForDms|系统规定参数。取值：DescribeDBInstanceForDms。 |
|Host|String|是|gp-xxxxxxxxxxxxxxxxx.gpdbmaster.singapore.rds.aliyuncs.com|连接地址DNS域名，不包含端口。 |
|Port|Long|是|5432|端口。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|"200"|内部返回码。 |
|Count|Long|1|实例数量。 |
|HttpStatusCode|Integer|200|通用http返回码。 |
|Instance|Struct| |实例详细信息。 |
|AliUid|String|1234566788909|阿里云账户ID。 |
|Bid|String|26842|区分示例值是金融云、政务云或公有云。 |
|ConnectionString|String|gp-xxxxxxxxxxxxxxxx-master.gpdbmaster.singapore.rds.aliyuncs.com|实例连接串地址。 |
|DbInstanceName|String|gp-xxxxxxxxxxxx|实例ID。 |
|DbType|String|gpdb|数据库类型。 |
|Description|String|”“|描述信息。 |
|InstanceNetworkType|String|vpc|实例网络类型。 |
|Port|String|5432|实例连接端口号。 |
|Region|String|ap-southeast-1|Region地址。 |
|VSwitchId|String|vsw-xxxxxxxxxxxxxxxxx|VSwitch的ID。 |
|Version|String|6.0|实例版本。 |
|VpcCloudInstanceId|String|gp-xxxxxxxxxxxxxxxxx-master-202103122033|VPC实例的ID。 |
|VpcId|String|vpc-xxxxxxxxxxxxxxxxx|VPC的ID。 |
|VpcIp|String|172.16.0.140|VPC的IP地址。 |
|Message|String|""|失败信息详情。 |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|请求ID。 |
|Success|Boolean|true|是否成功。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeDBInstanceForDms
&Host=gp-xxxxxxxx.gpdbmaster.singapore.rds.aliyuncs.com
&Port=5432
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<Message/>
<RequestId>B4CAF581-2AC7-41AD-8940-D56DF7AADF5B</RequestId>
<Instance>
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
</Instance>
<HttpStatusCode>200</HttpStatusCode>
<Count>1</Count>
<Code>200</Code>
<Success>true</Success>
```

`JSON`格式

```
{
  "Message": "",
  "RequestId": "B4CAF581-2AC7-41AD-8940-D56DF7AADF5B",
  "Instance": {
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
  },
  "HttpStatusCode": 200,
  "Count": 1,
  "Code": "200",
  "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

