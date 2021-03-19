# DescribeAvailableResources

调取DescribeAvailableResources接口获取可用资源信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeAvailableResources&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeAvailableResources|系统规定参数。取值：DescribeAvailableResources。 |
|Region|String|是|cn-hangzhou|地域信息。 |
|ZoneId|String|是|cn-hangzhou-h|可用区ID。 |
|ChargeType|String|否|PostPaid|付费类型。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RegionId|String|cn-hangzhou|地域信息。 |
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|请求ID。 |
|Resources|Array of Resource| |资源信息。 |
|SupportedEngines|Array of SupportedEngine| |支持的引擎。 |
|Mode|String|ECS|模式：ECS或物理机。 |
|SupportedEngineVersion|String|6.0|支持的引擎版本。 |
|SupportedInstanceClasses|Array of SupportedInstanceClass| |支持的规格。 |
|Description|String|单segment节点16核配置，含128GB 内存|Segment规格描述。 |
|DisplayClass|String|4C32G|Segment规格信息。 |
|InstanceClass|String|4C32G|Segment规格信息。 |
|NodeCount|Struct| |Segment节点数信息。 |
|MaxCount|String|128|Segment最大节点数。 |
|MinCount|String|2|Segment最小节点数。 |
|Step|String|2|增加节点时的步长。 |
|StorageSize|Struct| |Segment存储容量。 |
|MaxCount|String|1000|Segment最大存储容量。 |
|MinCount|String|50|Segment最小存储容量。 |
|Step|String|50|Segment存储扩容步长。 |
|StorageType|String|cloud\_ssd|存储类型。 |
|ZoneId|String|cn-hangzhou-h|可用区ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeAvailableResources
&<公共请求参数>
&Region=cn-hangzhou
&ZoneId=cn-hangzhou-h
&ChargeType=PostPaid
```

正常返回示例

`XML`格式

```
<RequestId>175B6B3B-E6F9-4158-824C-3FA43BC54193</RequestId>
<RegionId>cn-hangzhou</RegionId>
<Resources>
    <ZoneId>cn-hangzhou-h</ZoneId>
    <SupportedEngines>
        <SupportedInstanceClasses>
            <DisplayClass>1x2C SSD</DisplayClass>
            <Description>SSD 单个计算组含2个segment节点，单segment节点1核配置，含8GB 内存/ 80GB 有效存储空间/ 160GB 双副本总存储空间</Description>
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

`JSON`格式

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
              "Description": "SSD 单个计算组含2个segment节点，单segment节点1核配置，含8GB 内存/ 80GB 有效存储空间/ 160GB 双副本总存储空间",
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

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

