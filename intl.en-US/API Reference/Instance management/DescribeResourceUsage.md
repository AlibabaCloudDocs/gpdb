# DescribeResourceUsage

## Description

Queries the disk space usage of an instance.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to DescribeResourceUsage.|
|DBInstanceId|String|Yes|The ID of the instance.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|
|DBInstanceId|String|The ID of the instance.|
|Engine|String|The database engine used by the instance.|
|DiskUsed|Integer|The total space occupied by data files and log files. Unit: bytes. The value -1 indicates that no space is occupied by data or log files.|
|DataSize|Integer|The space occupied by data files. Unit: bytes. The value -1 indicates that no space is occupied by data files.|
|LogSize|Integer|The space occupied by log files. Unit: bytes. The value -1 indicates that no space is occupied by log files.|
|BackupSize|Integer|The space occupied by backup files. Unit: bytes. The value -1 indicates that no space is occupied by backup files.|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeResourceUsage
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>

```

## Sample responses

**XML format**

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

**JSON format**

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

