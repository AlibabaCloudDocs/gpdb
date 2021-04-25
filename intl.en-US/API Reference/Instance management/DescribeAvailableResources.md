# DescribeAvailableResources

You can call this operation to query the available resources.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeAvailableResources&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeAvailableResources|The operation that you want to perform. Set the value to DescribeAvailableResources. |
|Region|String|Yes|cn-hangzhou|The ID of the region. |
|ZoneId|String|Yes|cn-hangzhou-h|The ID of the zone. |
|ChargeType|String|No|PostPaid|The billing method. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RegionId|String|cn-hangzhou|The ID of the region. |
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|The ID of the request. |
|Resources|Array of Resource| |Details about the resources. |
|SupportedEngines|Array of SupportedEngine| |Details about the available database engines. |
|Mode|String|ECS|The type of the machine where the resources are deployed. Valid values: ECS instance and physical server. |
|SupportedEngineVersion|String|6.0|The available version of the database engine. |
|SupportedInstanceClasses|Array of SupportedInstanceClass| |The available specifications of the instance. |
|Description|String|16 cores and 128 GB memory for a single compute node|The specifications of the compute node. |
|DisplayClass|String|4C32G|The type of the compute node. |
|InstanceClass|String|4C32G|The type family of the compute node. |
|NodeCount|Struct| |Details about the number of compute nodes. |
|MaxCount|String|128|The maximum number of compute nodes. |
|MinCount|String|2|The minimum number of compute nodes. |
|Step|String|2|The step size of each number change of compute nodes. |
|StorageSize|Struct| |Details about the storage capacity of the compute node. |
|MaxCount|String|1000|The maximum storage capacity of the compute node. |
|MinCount|String|50|The minimum storage capacity of the compute node. |
|Step|String|50|The step size of each storage capacity change of the compute node. |
|StorageType|String|cloud\_ssd|The type of the disk. |
|ZoneId|String|cn-hangzhou-h|The zone ID of the instance. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeAvailableResources
&<Common request parameters>
&Region=cn-hangzhou
&ZoneId=cn-hangzhou-h
&ChargeType=PostPaid
```

Sample success responses

`XML` format

```
<RequestId>175B6B3B-E6F9-4158-824C-3FA43BC54193</RequestId>
<RegionId>cn-hangzhou</RegionId>
<Resources>
    <ZoneId>cn-hangzhou-h</ZoneId>
    <SupportedEngines>
        <SupportedInstanceClasses>
            <DisplayClass>1x2C SSD</DisplayClass>
            <Description>The disk type is SSD. The instance has two compute nodes. Each compute node has one core, 8 GB memory, and 80 GB space capacity. The total dual-copy storage capacity is 160 GB.</Description>
            <StorageType>SSD</StorageType>
            <NodeCount>
                <MaxCount>128</MaxCount>
                <Step>2</Step>
                <MinCount>2</MinCount>
            </NodeCount>
            <StorageSize/>
            <InstanceClass>gpdb.group.segsdx2</InstanceClass>
        </SupportedInstanceClasses>
        <Mode>physical</Mode>
        <SupportedEngineVersion>6.0</SupportedEngineVersion>
    </SupportedEngines>
</Resources>
```

`JSON` format

```
{
  "RequestId": "175B6B3B-E6F9-4158-824C-3FA43BC54193",
  "RegionId": "cn-hangzhou",
  "Resources": [
    {
      "ZoneId": "cn-hangzhou-h",
      "SupportedEngines": [
        {
          "SupportedInstanceClasses": [
            {
              "DisplayClass": "1x2C SSD",
              "Description": "The disk type is SSD. The instance has two compute nodes and 80 GB space capacity. Each compute node has one core and 8 GB memory. The total dual-copy storage capacity is 160 GB.",
              "StorageType": "SSD",
              "NodeCount": {
                "MaxCount": "128",
                "Step": "2",
                "MinCount": "2"
              },
              "StorageSize": {},
              "InstanceClass": "gpdb.group.segsdx2"
            }
          ],
          "Mode": "physical",
          "SupportedEngineVersion": "6.0"
        }
      ]
    }
  ]
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

