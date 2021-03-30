# DescribeDBInstancePerformance

调用DescribeDBInstancePerformance查看某个实例在某个时间段内指定性能参数的性能监控数据。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstancePerformance&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeDBInstancePerformance|系统规定参数。取值：DescribeDBInstancePerformance。 |
|DBInstanceId|String|是|gp-xxxxxxxx|实例ID。 |
|EndTime|String|是|2018-07-09T03:47Z|查询结束时间，格式如：2018-06-11T16:00Z。 |
|Key|String|是|CpuUsage,MemoryUsage,Gpdb\_SpaceUsage,Gpdb\_IOPS,Gpdb\_session|性能指标，多个指标用英文半角“,”分隔，见[性能参数表](~~86943~~)。 |
|StartTime|String|是|2018-07-08T03:47Z|查询开始时间，格式如：2018-06-11T15:00Z。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|DBInstanceId|String|gp-xxxxxxxxxx|实例ID。 |
|EndTime|String|2018-07-09T03:47Z|查询结束时间，格式：`YYYY-MM-DDTHH:mmZ`，如2018-05-30T03:29Z，大于查询开始时间。 |
|Engine|String|gpdb|数据库类型。 |
|PerformanceKeys|List|\{ "Key": "MemoryUsage", "GroupValues": \[\{ "Name": "7198315-1530522046144-1", "Values": "" \}, \{ "Name": "7198315-1530522046144-0", "Values": "" \}, \{"Values": "", "Name": "master" \} \], "Unit": "%", "ValueFormat": "mem\_usage"\}|数组格式：\{perf1, perf2, perf3, …\}。 |
|RequestId|String|5E85244A-AB47-46A3-A3AD-5F307DCB407E|请求ID。 |
|StartTime|String|2018-07-08T03:47Z|查询开始时间，格式：`YYYY-MM-DDTHH:mmZ`，如2018-05-30T03:29Z。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstancePerformance
&DBInstanceId=gp-xxxxxxxx
&EndTime=2018-07-09T03:47Z
&Key=CpuUsage,MemoryUsage,Gpdb_SpaceUsage,Gpdb_IOPS,Gpdb_session
&StartTime=2018-07-08T03:47Z
&<公共请求参数>
```

正常返回示例

`XML`格式

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

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

