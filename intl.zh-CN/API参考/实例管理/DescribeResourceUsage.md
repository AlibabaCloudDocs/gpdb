# DescribeResourceUsage

调用DescribeResourceUsage查看实例的资源利用情况。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeResourceUsage&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeResourceUsage|系统规定参数。取值：DescribeResourceUsage。 |
|DBInstanceId|String|是|gp-xxxxxxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|860FBA9E-BFFD-493E-91A1-2349B9D74AE7|请求ID。 |
|DBInstanceId|String|gp-xxxxxxxx|实例ID。 |
|Engine|String|gpdb|数据库类型。 |
|DiskUsed|Long|285212672|已用空间（DataSize+LogSize），单位：Byte，-1表示没有数据。 |
|DataSize|Long|83886080|数据文件占用空间，单位：Byte，-1表示没有数据。 |
|LogSize|Long|201326592|日志占用空间，单位：Byte，-1表示没有数据。 |
|BackupSize|Long|26624|备份占用空间，单位：Byte，-1表示没有数据。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeResourceUsage
&DBInstanceId=gp-xxxxxxxxxxx
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DescribeResourceUsageResponse>
          <DBInstanceId>gp-xxxxxxx</DBInstanceId>
          <RequestId>860FBA9E-BFFD-493E-91A1-2349B9D74AE7</RequestId>
          <LogSize>201326592</LogSize>
          <DataSize>83886080</DataSize>
          <BackupSize>26624</BackupSize>
          <Engine>gpdb</Engine>
          <DiskUsed>285212672</DiskUsed>
</DescribeResourceUsageResponse>
```

`JSON` 格式

```
{
    "DBInstanceId":"gp-xxxxxxx",
    "RequestId":"860FBA9E-BFFD-493E-91A1-2349B9D74AE7",
    "LogSize":201326592,
    "DataSize":83886080,
    "BackupSize":26624,
    "Engine":"gpdb",
    "DiskUsed":285212672
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

