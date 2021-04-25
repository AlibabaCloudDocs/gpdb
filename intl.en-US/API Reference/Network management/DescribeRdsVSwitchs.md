# DescribeRdsVSwitchs

You can call this operation to query the information about vSwitches within a specific VPC.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeRdsVSwitchs&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|No|DescribeRdsVSwitchs|The operation that you want to perform. Set the value to DescribeRdsVSwitchs. |
|VpcId|String|No|vpc-xxxxxx|The ID of the VPC. |
|ZoneId|String|No|cn-hangzhou-h|The ID of the zone. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|The ID of the request. |
|VSwitches|Struct| |Details about the vSwitches. |
|VSwitch|Array of VSwitch| |The information of the vSwitch. |
|AliUid|String|123|The ID of the Alibaba Cloud account. |
|Bid|String|26842|The ID of the account group, indicating that the account belongs to Alibaba Finance Cloud, Alibaba Gov Cloud, or Alibaba Cloud. |
|CidrBlock|String|192.168.1.0/24|The CIDR block of the vSwitch. |
|GmtCreate|String|2020-11-22T22:22:22Z|The time when the vSwitch was created. |
|GmtModified|String|2020-11-22T22:22:22Z|The last time when the vSwitch was modified. |
|IsDefault|Boolean|true|Indicates whether the vSwitch is the default vSwitch. |
|IzNo|String|123|The ID of the zone. |
|RegionNo|String|cn-hangzhou-i|The ID of the region. |
|Status|String|Available|The status of the vSwitch. |
|VSwitchId|String|vsw-xxx|The ID of the vSwitch. |
|VSwitchName|String|vsw-name|The name of the vSwitch. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeRdsVSwitchs
&<Common request parameters>
&VpcId=vpc-xxx
&ZoneId=cn-hangzhou-h
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

