# DescribeDBInstancePerformance

You can call this operation to query the specified performance metrics of an instance over a specified time range.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstancePerformance&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|DBInstanceId|String|Yes|gp-xxxxxxxx|The ID of the instance. |
|EndTime|String|Yes|2018-07-09T03:47Z|The end of the time range to query. Example: 2018-06-11T16:00Z. |
|Key|String|Yes|CpuUsage,MemoryUsage,Gpdb\_SpaceUsage,Gpdb\_IOPS,Gpdb\_session|The performance metrics. Separate multiple performance metrics with commas \(,\). For more information, see [Performance parameters](https://help.aliyun.com/document_detail/86943.html?spm=a2c4g.11186623.2.13.4fdd5d6bmjXyc6#concept-g5l-d3m-q2b). |
|StartTime|String|Yes|2018-07-08T03:47Z|The beginning of the time range to query. Example: 2018-06-11T15:00Z. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5E85244A-AB47-46A3-A3AD-5F307DCB407E|The ID of the request. |
|DBInstanceId|String|gp-xxxxxxxxxx|The ID of the instance. |
|Engine|String|gpdb|The database engine of the instance. |
|StartTime|String|2018-07-08T03:47Z|The beginning of the time range over which the query was performed. The time is displayed in the `YYYY-MM-DDTHH:mmZ` format. Example: 2018-05-30T03:29Z. |
|EndTime|String|2018-07-09T03:47Z|The end of the time range over which the query was performed. The time is displayed in the `YYYY-MM-DDTHH:mmZ`format. The end time is later than the start time. Example: 2018-05-30T03:29Z. |
|PerformanceKeys|List|\{ "Key": "MemoryUsage", "GroupValues": \[\{ "Name": "7198315-1530522046144-1", "Values": "" \}, \{ "Name": "7198315-1530522046144-0", "Values": "" \}, \{"Values": "", "Name": "master" \} \], "Unit": "%", "ValueFormat": "mem\_usage"\}|Details about the performance metrics Format: \{perf1, perf2, perf3, …\}. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstancePerformance
&DBInstanceId=gp-xxxxxxxx
&EndTime=2018-07-09T03:47Z
&Key=CpuUsage,MemoryUsage,Gpdb_SpaceUsage,Gpdb_IOPS,Gpdb_session
&StartTime=2018-07-08T03:47Z
&<Common request parameters>
```

Sample success responses

`XML` format

```
<DescribeDBInstancePerformanceResponse> 
      <RequestId>5E85244A-AB47-46A3-A3AD-5F307DCB407E</RequestId>
      <DBInstanceId>gp-xxxxxxx</DBInstanceId>
      <PerformanceKeys>
            <Key>MemoryUsage</Key>
            <GroupValues>
                  <Name>7198315-1530522046144-1</Name>
                  <Values></Values>
            </GroupValues>
            <GroupValues>
                  <Name>7198315-1530522046144-0</Name>
                  <Values></Values>
            </GroupValues>
            <GroupValues>
                  <Values></Values>
                  <Name>master</Name>
            </GroupValues>
            <Unit>%</Unit>
            <ValueFormat>mem_usage</ValueFormat>
      </PerformanceKeys>
      <PerformanceKeys>
            <Key>Gpdb_session</Key>
            <GroupValues>
                  <Name>7198315-1530522046144-1</Name>
                  <Values></Values>
            </GroupValues>
            <GroupValues>
                  <Name>7198315-1530522046144-0</Name>
                  <Values></Values>
            </GroupValues>
            <GroupValues>
                  <Values></Values>
                  <Name>master</Name>
            </GroupValues>
            <Unit>int</Unit>
            <ValueFormat>conn_count</ValueFormat>
      </PerformanceKeys>
      <PerformanceKeys>
            <Key>CpuUsage</Key>
            <GroupValues>
                  <Name>7198315-1530522046144-1</Name>
                  <Values></Values>
            </GroupValues>
            <GroupValues>
                  <Name>7198315-1530522046144-0</Name>
                  <Values></Values>
            </GroupValues>
            <GroupValues>
                  <Values></Values>
                  <Name>master</Name>
            </GroupValues>
            <Unit>%</Unit>
            <ValueFormat>cpu_usage</ValueFormat>
      </PerformanceKeys>
      <PerformanceKeys>
            <Key>Gpdb_IOPS</Key>
            <GroupValues>
                  <Name>7198315-1530522046144-1</Name>
                  <Values></Values>
            </GroupValues>
            <GroupValues>
                  <Name>7198315-1530522046144-0</Name>
                  <Values></Values>
            </GroupValues>
            <GroupValues>
                  <Values></Values>
                  <Name>master</Name>
            </GroupValues>
            <Unit>int</Unit>
            <ValueFormat>data_iops&amp;write_iops&amp;read_iops</ValueFormat>
      </PerformanceKeys>
      <PerformanceKeys>
            <Key>Gpdb_SpaceUsage</Key>
            <GroupValues>
                  <Name>7198315-1530522046144-1</Name>
                  <Values></Values>
            </GroupValues>
            <GroupValues>
                  <Name>7198315-1530522046144-0</Name>
                  <Values></Values>
            </GroupValues>
            <GroupValues>
                  <Values></Values>
                  <Name>master</Name>
            </GroupValues>
            <Unit>Byte</Unit>
            <ValueFormat>space</ValueFormat>
      </PerformanceKeys>
      <EndTime>2018-07-09T03:47Z</EndTime>
      <StartTime>2018-07-08T03:47Z</StartTime>
      <Engine>gpdb</Engine>
</DescribeDBInstancePerformanceResponse>
```

`JSON` format

```
{
        "RequestId": "5E85244A-AB47-46A3-A3AD-5F307DCB407E",
        "DBInstanceId": "gp-xxxxxxx",
        "PerformanceKeys": [
            {
                "Key": "MemoryUsage",
                "GroupValues": [
                    {
                        "Name": "7198315-1530522046144-1",
                        "Values": ""
                    },
                    {
                        "Name": "7198315-1530522046144-0",
                        "Values": ""
                    },
                    {
                        "Values": "",
                        "Name": "master"
                    }
                ],
                "Unit": "%",
                "ValueFormat": "mem_usage"
            },
            {
                "Key": "Gpdb_session",
                "GroupValues": [
                    {
                        "Name": "7198315-1530522046144-1",
                        "Values": ""
                    },
                    {
                        "Name": "7198315-1530522046144-0",
                        "Values": ""
                    },
                    {
                        "Values": "",
                        "Name": "master"
                    }
                ],
                "Unit": "int",
                "ValueFormat": "conn_count"
            },
            {
                "Key": "CpuUsage",
                "GroupValues": [
                    {
                        "Name": "7198315-1530522046144-1",
                        "Values": ""
                    },
                    {
                        "Name": "7198315-1530522046144-0",
                        "Values": ""
                    },
                    {
                        "Values": "",
                        "Name": "master"
                    }
                ],
                "Unit": "%",
                "ValueFormat": "cpu_usage"
            },
            {
                "Key": "Gpdb_IOPS",
                "GroupValues": [
                    {
                        "Name": "7198315-1530522046144-1",
                        "Values": ""
                    },
                    {
                        "Name": "7198315-1530522046144-0",
                        "Values": ""
                    },
                    {
                        "Values": "",
                        "Name": "master"
                    }
                ],
                "Unit": "int",
                "ValueFormat": "data_iops&amp;write_iops&amp;read_iops"
            },
            {
                "Key": "Gpdb_SpaceUsage",
                "GroupValues": [
                    {
                        "Name": "7198315-1530522046144-1",
                        "Values": ""
                    },
                    {
                        "Name": "7198315-1530522046144-0",
                        "Values": ""
                    },
                    {
                        "Values": "",
                        "Name": "master"
                    }
                ],
                "Unit": "Byte",
                "ValueFormat": "space"
            }
        ],
        "EndTime": "2018-07-09T03:47Z",
        "StartTime": "2018-07-08T03:47Z",
        "Engine": "gpdb"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

