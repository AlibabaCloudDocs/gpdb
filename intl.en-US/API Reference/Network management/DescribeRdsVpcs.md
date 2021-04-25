# DescribeRdsVpcs

You can call this operation to query the information about VPCs in a specific zone.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeRdsVpcs&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|No|DescribeRdsVpcs|The operation that you want to perform. Set the value to DescribeRdsVpcs. |
|ZoneId|String|No|cn-hangzhou-h|The ID of the zone. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|The ID of the request. |
|Vpcs|Struct| |Details about the VPCs. |
|Vpc|Array of Vpc| |The information of the VPC. |
|AliUid|String|123|The ID of the Alibaba Cloud account. |
|Bid|String|26842|The ID of the account group, indicating that the account belongs to Alibaba Finance Cloud, Alibaba Gov Cloud, or Alibaba Cloud. |
|CidrBlock|String|192.168.1.0/24|The CIDR block of the VPC. |
|GmtCreate|String|2020-11-22T11:22:33Z|The time when the VPC was created. |
|GmtModified|String|2020-12-22T11:22:33Z|The last time when the VPC was modified. |
|IsDefault|Boolean|true|Indicates whether the VPC is the default VPC. |
|RegionNo|String|cn-hangzhou|The ID of the region. |
|Status|String|Available|The status of the VPC. |
|VSwitchs|Array of VSwitch| |The information of the vSwitch. |
|CidrBlock|String|192.168.0.0/24|The CIDR block of the vSwitch. |
|GmtCreate|String|2020-12-22T11:22:33Z|The time when the VPC was created. |
|GmtModified|String|2020-12-24T11:22:33Z|The last time when the VPC was modified. |
|IsDefault|Boolean|true|Indicates whether the vSwitch is the default vSwitch. |
|IzNo|String|cn-hangzhou-i|The ID of the zone. |
|Status|String|Available|The status of the vSwitch. |
|VSwitchId|String|vsw-xxx|The ID of the vSwitch. |
|VSwitchName|String|vsw-name|The name of the vSwitch. |
|VpcId|String|vpc-xxx|The ID of the VPC. |
|VpcName|String|vpc-name|The name of the VPC. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeRdsVpcs
&<Common request parameters>
&ZoneId=cn-hangzhou-x
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

