# DescribeRdsVpcs

调取DescribeRdsVpcs接口获取VPC信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeRdsVpcs&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|否|DescribeRdsVpcs|系统规定参数，取值： DescribeRdsVpcs。 |
|ZoneId|String|否|cn-hangzhou-h|可用区ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|请求ID。 |
|Vpcs|Struct| |VPC信息。 |
|Vpc|Array of Vpc| |VPC信息。 |
|AliUid|String|123|阿里云账户ID。 |
|Bid|String|26842|区分这个账号是金融云，政务云或公有云的。 |
|CidrBlock|String|192.168.1.0/24|交换机网段。 |
|GmtCreate|String|2020-11-22T11:22:33Z|创建时间。 |
|GmtModified|String|2020-12-22T11:22:33Z|最后修改时间。 |
|IsDefault|Boolean|true|是否是默认VPC。 |
|RegionNo|String|cn-hangzhou|地域。 |
|Status|String|Available|VPC状态。 |
|VSwitchs|Array of VSwitch| |交换机信息。 |
|CidrBlock|String|192.168.0.0/24|交换机网段。 |
|GmtCreate|String|2020-12-22T11:22:33Z|创建时间。 |
|GmtModified|String|2020-12-24T11:22:33Z|最后修改时间。 |
|IsDefault|Boolean|true|是否是默认交换机。 |
|IzNo|String|cn-hangzhou-i|可用区。 |
|Status|String|Available|交换机状态。 |
|VSwitchId|String|vsw-xxx|交换机ID。 |
|VSwitchName|String|vsw-name|交换机名称。 |
|VpcId|String|vpc-xxx|VPC的ID。 |
|VpcName|String|vpc-name|VPC的名称。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeRdsVpcs
&<公共请求参数>
&ZoneId=cn-hangzhou-x
```

正常返回示例

`XML`格式

```
<Vpcs>
    <Vpc>
        <IsDefault>false</IsDefault>
        <RegionNo>cn-hangzhou</RegionNo>
        <VSwitchs>
            <IsDefault>false</IsDefault>
            <Status>Available</Status>
            <IzNo>cn-hangzhou-i</IzNo>
            <VSwitchId>vsw-xxxxxx</VSwitchId>
            <CidrBlock>192.168.1.0/24</CidrBlock>
            <VSwitchName>vsw-hz-i</VSwitchName>
        </VSwitchs>
        <VpcId>vpc-xxxxxx</VpcId>
        <CidrBlock>192.168.0.0/16</CidrBlock>
        <VpcName>xxxxxx</VpcName>
    </Vpc>
</Vpcs>
<RequestId>92C73FC0-2B2C-4230-8A20-8EEDE655111D</RequestId>
```

`JSON`格式

```
{
  "Vpcs": {
    "Vpc": [
      {
        "IsDefault": false,
        "RegionNo": "cn-hangzhou",
        "VSwitchs": [
          {
            "IsDefault": false,
            "Status": "Available",
            "IzNo": "cn-hangzhou-i",
            "VSwitchId": "vsw-xxxxxx",
            "CidrBlock": "192.168.1.0/24",
            "VSwitchName": "vsw-hz-i"
          }
        ],
        "VpcId": "vpc-xxxxxx",
        "CidrBlock": "192.168.0.0/16",
        "VpcName": "xxxxxx"
      }
    ]
  },
  "RequestId": "92C73FC0-2B2C-4230-8A20-8EEDE655111D"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

