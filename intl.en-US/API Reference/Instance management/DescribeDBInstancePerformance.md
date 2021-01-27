# DescribeDBInstancePerformance

## Description

Queries specified performance metrics for an instance over a specific time range.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to DescribeDBInstancePerformance.|
|DBInstanceId|String|Yes|The ID of the instance.|
|Key|String|Yes|The performance metric you want to query. Separate multiple performance metrics with commas \(,\). For more information, see [Performance parameters](/intl.en-US/API Reference/Appendix/Performance parameters.md).|
|StartTime|String|Yes|The start time of the query, such as 2018-06-11T15:00Z.|
|EndTime|String|Yes|The end time of the query, such as 2018-06-11T16:00Z.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|
|DBInstanceId|String|The ID of the instance.|
|Engine|String|The database engine used by the instance.|
|StartTime|String|The start time of the query. The time is in the `YYYY-MM-DDTHH:mmZ` format, such as 2018-05-30T03:29Z.|
|EndTime|String|The end time of the query. The time is in the `YYYY-MM-DDTHH:mmZ` format, such as 2018-05-30T03:29Z. The end time must be later than the start time.|
|PerformanceKeys|List<PerformanceKey\>|An array consisting of performance metrics in the \{perf1, perf2, perf3, ...\} format.|

|Parameter|Type|Description|
|---------|----|-----------|
|Key|String|The performance metric.|
|Unit|String|The unit of the performance metric.|
|ValueFormat|String|The format of the performance metric value. If the performance metric has multiple value fields, they are separated with ampersands \(&\), such as `com_delete&com_insert&com_insert_select&com_replace`. The value fields specified in the ValueFormat parameter correspond to the values specified in the Value parameter.|
|GroupValues|List<GroupValue\>|An array consisting of metric values in groups in the \{value1, value2, ...\} format.|

|Parameter|Type|Description|
|---------|----|-----------|
|Name|String|The name of the group.|
|Values|List<Value\>|An array consisting of metric values in the \{value1, value2, ...\} format.|

|Parameter|Type|Description|
|---------|----|-----------|
|Value|String|The value of the performance metric.|
|Date|String|The time when the metric value was recorded. Unit: milliseconds.|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstancePerformance
&DBInstanceId=gp-xxxxxxx
&key=CpuUsage,MemoryUsage,Gpdb_SpaceUsage,Gpdb_IOPS,Gpdb_session
&EndTime=2018-07-09T03:43Z
&StartTime=2018-07-08T03:43Z
&<Common request parameters>
```

## Sample responses

**XML format**

```
<DescribeDBInstancePerformanceResponse> 
    <RequestId>5E85244A-AB47-46A3-A3AD-5F307DCB407E</RequestId>
	<DBInstanceId>gp-xxxxxxx</DBInstanceId>
	<PerformanceKeys>
		<Key>MemoryUsage</Key>
		<GroupValues>
			<Name>7198315-1530522046144-1</Name>
			<Values />
		</GroupValues>
		<GroupValues>
			<Name>7198315-1530522046144-0</Name>
			<Values />
		</GroupValues>
		<GroupValues>
			<Values />
			<Name>master</Name>
		</GroupValues>
		<Unit>%</Unit>
		<ValueFormat>mem_usage</ValueFormat>
	</PerformanceKeys>
	<PerformanceKeys>
		<Key>Gpdb_session</Key>
		<GroupValues>
			<Name>7198315-1530522046144-1</Name>
			<Values />
		</GroupValues>
		<GroupValues>
			<Name>7198315-1530522046144-0</Name>
			<Values />
		</GroupValues>
		<GroupValues>
			<Values />
			<Name>master</Name>
		</GroupValues>
		<Unit>int</Unit>
		<ValueFormat>conn_count</ValueFormat>
	</PerformanceKeys>
	<PerformanceKeys>
		<Key>CpuUsage</Key>
		<GroupValues>
			<Name>7198315-1530522046144-1</Name>
			<Values />
		</GroupValues>
		<GroupValues>
			<Name>7198315-1530522046144-0</Name>
			<Values />
		</GroupValues>
		<GroupValues>
			<Values />
			<Name>master</Name>
		</GroupValues>
		<Unit>%</Unit>
		<ValueFormat>cpu_usage</ValueFormat>
	</PerformanceKeys>
	<PerformanceKeys>
		<Key>Gpdb_IOPS</Key>
		<GroupValues>
			<Name>7198315-1530522046144-1</Name>
			<Values />
		</GroupValues>
		<GroupValues>
			<Name>7198315-1530522046144-0</Name>
			<Values />
		</GroupValues>
		<GroupValues>
			<Values />
			<Name>master</Name>
		</GroupValues>
		<Unit>int</Unit>
		<ValueFormat>data_iops&amp;write_iops&amp;read_iops</ValueFormat>
	</PerformanceKeys>
	<PerformanceKeys>
		<Key>Gpdb_SpaceUsage</Key>
		<GroupValues>
			<Name>7198315-1530522046144-1</Name>
			<Values />
		</GroupValues>
		<GroupValues>
			<Name>7198315-1530522046144-0</Name>
			<Values />
		</GroupValues>
		<GroupValues>
			<Values />
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

**JSON format**

```
{
        "RequestId":"5E85244A-AB47-46A3-A3AD-5F307DCB407E",
        "DBInstanceId":"gp-xxxxxxx",
        "PerformanceKeys":[
            {
                "Key":"MemoryUsage",
                "GroupValues":[
                    {
                        "Name":"7198315-1530522046144-1",
                        "Values":Array[288]
                    },
                    {
                        "Name":"7198315-1530522046144-0",
                        "Values":Array[288]
                    },
                    {
                        "Values":Array[288],
                        "Name":"master"
                    }
                ],
                "Unit":"%",
                "ValueFormat":"mem_usage"
            },
            {
                "Key":"Gpdb_session",
                "GroupValues":[
                    {
                        "Name":"7198315-1530522046144-1",
                        "Values":Array[288]
                    },
                    {
                        "Name":"7198315-1530522046144-0",
                        "Values":Array[288]
                    },
                    {
                        "Values":Array[288],
                        "Name":"master"
                    }
                ],
                "Unit":"int",
                "ValueFormat":"conn_count"
            },
            {
                "Key":"CpuUsage",
                "GroupValues":[
                    {
                        "Name":"7198315-1530522046144-1",
                        "Values":Array[288]
                    },
                    {
                        "Name":"7198315-1530522046144-0",
                        "Values":Array[288]
                    },
                    {
                        "Values":Array[288],
                        "Name":"master"
                    }
                ],
                "Unit":"%",
                "ValueFormat":"cpu_usage"
            },
            {
                "Key":"Gpdb_IOPS",
                "GroupValues":[
                    {
                        "Name":"7198315-1530522046144-1",
                        "Values":Array[288]
                    },
                    {
                        "Name":"7198315-1530522046144-0",
                        "Values":Array[288]
                    },
                    {
                        "Values":Array[288],
                        "Name":"master"
                    }
                ],
                "Unit":"int",
                "ValueFormat":"data_iops&write_iops&read_iops"
            },
            {
                "Key":"Gpdb_SpaceUsage",
                "GroupValues":[
                    {
                        "Name":"7198315-1530522046144-1",
                        "Values":Array[288]
                    },
                    {
                        "Name":"7198315-1530522046144-0",
                        "Values":Array[288]
                    },
                    {
                        "Values":Array[288],
                        "Name":"master"
                    }
                ],
                "Unit":"Byte",
                "ValueFormat":"space"
            }
        ],
        "EndTime":"2018-07-09T03:47Z",
        "StartTime":"2018-07-08T03:47Z",
        "Engine":"gpdb"
    }

```

