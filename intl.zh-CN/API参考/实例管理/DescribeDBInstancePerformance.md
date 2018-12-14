# DescribeDBInstancePerformance {#concept_asc_ccm_q2b .concept}

## 描述 { .section}

查看某个用户实例在某个时间段内指定性能参数的性能监控数据。

## 请求参数 { .section}

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](intl.zh-CN/API参考/公共参数.md#)。|
|Action|String|是|系统规定参数，取值：DescribeDBInstancePerformance。|
|DBInstanceId|String|是|实例名。|
|Key|String|是|性能指标，多个指标用英文半角“,”分隔，见[性能参数表](intl.zh-CN/API参考/附录/性能参数表.md#)。|
|StartTime|String|是|查询开始时间，格式如：2018-06-11T15:00Z。|
|EndTime|String|是|查询结束时间，格式如：2018-06-11T16:00Z。|

## 返回参数 { .section}

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>|无|详见[公共返回参数](intl.zh-CN/API参考/公共参数.md#section_apd_1rv_3bb)。|
|DBInstanceId|String|实例名。|
|Engine|String|数据库类型。|
|StartTime|String|查询开始时间，格式：`YYYY-MM-DDTHH:mmZ`，如2018-05-30T03:29Z。|
|EndTime|String|查询结束时间，格式：`YYYY-MM-DDTHH:mmZ`，如2018-05-30T03:29Z，大于查询开始时间。|
|PerformanceKeys|List<PerformanceKey\>|数组格式：\{perf1, perf2, perf3, …\}。|

|名称|类型|描述|
|--|--|--|
|Key|String|性能参数。|
|Unit|String|展示单位。|
|ValueFormat|String|值的格式。如果有多个值，以“&”分隔，例如`com_delete&com_insert&com_insert_select&com_replace`。Value与之对应。|
|GroupValues|List<GroupValue\>|数组格式：\{value1, value2, …\}。|

|名称|类型|描述|
|--|--|--|
|Name|String|组名。|
|Values|List<Value\>|数组格式：\{value1, value2, …\}。|

|名称|类型|描述|
|--|--|--|
|Value|String|性能值。|
|Date|String|计算日期，单位为毫秒。|

## 请求示例 { .section}

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstancePerformance
&DBInstanceId=gp-xxxxxxx
&key=CpuUsage,MemoryUsage,Gpdb_SpaceUsage,Gpdb_IOPS,Gpdb_session
&EndTime=2018-07-09T03:43Z
&StartTime=2018-07-08T03:43Z
&<公共请求参数>
```

## 返回示例 { .section}

**XML格式**

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

**JSON格式**

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

