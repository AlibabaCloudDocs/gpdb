# DescribeRdsVSwitchs

调取DescribeRdsVSwitchs接口获取交换机信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeRdsVSwitchs&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|否|DescribeRdsVSwitchs|系统规定参数，取值DescribeRdsVSwitchs。 |
|VpcId|String|否|vpc-xxxxxx|VPC的ID。 |
|ZoneId|String|否|cn-hangzhou-h|可用区。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|请求ID。 |
|VSwitches|Struct| |交换机信息。 |
|VSwitch|Array of VSwitch| |交换机信息。 |
|AliUid|String|123|用户ID。 |
|Bid|String|26842|区分账户是属于金融云、政务云或公有云。 |
|CidrBlock|String|192.168.1.0/24|交换机网段。 |
|GmtCreate|String|2020-11-22T22:22:22Z|创建时间。 |
|GmtModified|String|2020-11-22T22:22:22Z|最后修改时间。 |
|IsDefault|Boolean|true|是否是默认交换机。 |
|IzNo|String|123|可用区。 |
|RegionNo|String|cn-hangzhou-i|地域。 |
|Status|String|Available|状态。 |
|VSwitchId|String|vsw-xxx|交换机ID。 |
|VSwitchName|String|vsw-name|交换机名称。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeRdsVSwitchs
&<公共请求参数>
&VpcId=vpc-xxx
&ZoneId=cn-hangzhou-h
```

正常返回示例

`XML`格式

```
<RequestId>DE2056E8-8B6E-4B6E-BA00-8776BB4934CF</RequestId>
<VSwitches>
    <VSwitch>
        <IsDefault>false</IsDefault>
        <Status>Available</Status>
        <IzNo>cn-hangzhou-i</IzNo>
        <VSwitchId>vsw-xxxxxx</VSwitchId>
        <CidrBlock>192.168.1.0/24</CidrBlock>
        <VSwitchName>vsw-xxxxx</VSwitchName>
    </VSwitch>
</VSwitches>
```

`JSON`格式

```
{
  "RequestId": "DE2056E8-8B6E-4B6E-BA00-8776BB4934CF",
  "VSwitches": {
    "VSwitch": [
      {
        "IsDefault": false,
        "Status": "Available",
        "IzNo": "cn-hangzhou-i",
        "VSwitchId": "vsw-xxxxxx",
        "CidrBlock": "192.168.1.0/24",
        "VSwitchName": "vsw-xxxxx"
      }
    ]
  }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

