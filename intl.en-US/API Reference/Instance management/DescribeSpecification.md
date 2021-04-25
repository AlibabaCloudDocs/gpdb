# DescribeSpecification

You can call this operation to query the specifications of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeSpecification&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeSpecification|The operation that you want to perform. Set the value to DescribeSpecification. |
|CpuCores|Integer|Yes|2|The number of CPU cores. |
|DBInstanceId|String|Yes|gp-xxx|The ID of the instance. |
|StorageType|String|Yes|cloud\_ssd|The type of the disk. |
|TotalNodeNum|Integer|Yes|4|The number of compute nodes. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|DBInstanceClass|Array of DBInstanceClass| |Details about the specifications of the compute node. |
|Text|String|gpdb.group.xxxx|The type family of the compute node. |
|Value|String|4x4C SSD|The type of the compute node. |
|DBInstanceGroupCount|Array of DBInstanceGroupCount| |Deatails about the compute nodes. |
|Text|String|2|The number of compute nodes. |
|Value|String|2|The number of compute nodes. |
|RequestId|String|C63CC586-984C-4012-B6F7-6EE52C73749A|The ID of the request. |
|StorageNotice|Array of StorageNotice| |Details about the storage capacity of the compute node. |
|Text|String|32 CPU Cores/64GB Memory/2560GB SSD Storage|The storage capacity of the compute node. |
|Value|String|32 CPU Cores/64GB Memory/2560GB SSD Storage|The storage capacity of the compute node. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeSpecification
&CpuCores=2
&DBInstanceId=gp-xxx
&StorageType=cloud_ssd
&TotalNodeNum=4
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

```
{"StorageNotice":[{"Value":"32 CPU Cores/64GB Memory/2560GB SSD Storage","Text":"32 CPU Cores/64GB Memory/2560GB SSD Storage"}],"RequestId":"C63CC586-984C-4012-B6F7-6EE52C73749A","DBInstanceClass":[{"Value":"4x4C SSD","Text":"gpdb.group.xxxx"}],"DBInstanceGroupCount":[{"Value":"2","Text":"2"}]}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

