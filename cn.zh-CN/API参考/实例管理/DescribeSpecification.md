# DescribeSpecification

调取DescribeSpecification接口获取规格信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeSpecification&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeSpecification|系统规定参数。取值：DescribeSpecification。 |
|CpuCores|Integer|是|2|CPU核数。 |
|DBInstanceId|String|是|gp-xxx|实例ID。 |
|StorageType|String|是|cloud\_ssd|存储类型。 |
|TotalNodeNum|Integer|是|4|Segment数量。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|DBInstanceClass|Array of DBInstanceClass| |Segment规格信息。 |
|Text|String|gpdb.group.xxxx|Segment规格码。 |
|Value|String|4x4C SSD|Segment规格值。 |
|DBInstanceGroupCount|Array of DBInstanceGroupCount| |计算组信息。 |
|Text|String|2|计算组数量。 |
|Value|String|2|计算组数量。 |
|RequestId|String|C63CC586-984C-4012-B6F7-6EE52C73749A|请求ID。 |
|StorageNotice|Array of StorageNotice| |计算组存储信息。 |
|Text|String|32 CPU Cores/64GB Memory/2560GB SSD Storage|计算组存储信息。 |
|Value|String|32 CPU Cores/64GB Memory/2560GB SSD Storage|计算组存储信息。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeSpecification
&CpuCores=2
&DBInstanceId=gp-xxx
&StorageType=cloud_ssd
&TotalNodeNum=4
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<StorageNotice>
    <Value>32 CPU Cores/64GB Memory/2560GB SSD Storage</Value>
    <Text>32 CPU Cores/64GB Memory/2560GB SSD Storage</Text>
</StorageNotice>
<RequestId>C63CC586-984C-4012-B6F7-6EE52C73749A</RequestId>
<DBInstanceClass>
    <Value>4x4C SSD</Value>
    <Text>gpdb.group.xxxx</Text>
</DBInstanceClass>
<DBInstanceGroupCount>
    <Value>2</Value>
    <Text>2</Text>
</DBInstanceGroupCount>
```

`JSON`格式

```
{"StorageNotice":[{"Value":"32 CPU Cores/64GB Memory/2560GB SSD Storage","Text":"32 CPU Cores/64GB Memory/2560GB SSD Storage"}],"RequestId":"C63CC586-984C-4012-B6F7-6EE52C73749A","DBInstanceClass":[{"Value":"4x4C SSD","Text":"gpdb.group.xxxx"}],"DBInstanceGroupCount":[{"Value":"2","Text":"2"}]}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

