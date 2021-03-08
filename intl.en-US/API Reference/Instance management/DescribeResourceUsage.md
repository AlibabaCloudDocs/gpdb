# DescribeResourceUsage

You can call this operation to query the disk space that is used by an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeResourceUsage&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeResourceUsage|The operation that you want to perform. Set the value to DescribeResourceUsage. |
|DBInstanceId|String|Yes|gp-xxxxxxxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|860FBA9E-BFFD-493E-91A1-2349B9D74AE7|The ID of the request. |
|DBInstanceId|String|gp-xxxxxxxx|The ID of the instance. |
|Engine|String|gpdb|The database engine of the instance. |
|DiskUsed|Long|285212672|The total space occupied by data and log files. Unit: bytes. -1 indicates that no space is used. |
|DataSize|Long|83886080|The disk space that is used to store data files. Unit: bytes. -1 indicates that no space is used. |
|LogSize|Long|201326592|The disk space that is used to store log files. Unit: bytes. -1 indicates that no space is used. |
|BackupSize|Long|26624|The disk space that is used to store backup files. Unit: bytes. -1 indicates no space is used. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeResourceUsage
&DBInstanceId=gp-xxxxxxxxxxx
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

