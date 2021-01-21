# DescribeSlowLogRecords

## 描述

用户可以查询某个时间段内某个用户实例的某个数据库的慢查询明细。

## 请求参数

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](/intl.zh-CN/API参考/公共参数.md)。|
|Action|String|是|系统规定参数，取值：DescribeSlowLogRecords。|
|DBInstanceId|String|是|实例名。|
|StartTime|String|是|查询开始日期，格式：`YYYY-MM-DDTHH:mmZ`，如2011-06-11T15:00Z。|
|EndTime|String|是|查询结束日期，不能小于查询开始日期，格式：`YYYY-MM-DDTHH:mmZ`，如2011-06-11 T16:00Z。|
|SQLId|Long|否|SQL ID。|
|DBName|String|否|数据库名称。|
|PageSize|Integer|否|每页记录数，取值：30/50/100；默认值：30。|
|PageNumber|Integer|否|页码，大于0且不超过Integer的最大值；默认值：1。|

## 返回参数

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>| |详见[公共返回参数](/intl.zh-CN/API参考/公共参数.mdsection_apd_1rv_3bb)。|
|Engine|String|数据库类型。|
|TotalRecordCount|Integer|总记录数。|
|PageNumber|Integer|页码。|
|PageRecordCount|Integer|本页SQL语句个数。|
|Items|List<SQLSlowRecord\>|由SQLSlowRecord组成的数组。|

|名称|类型|描述|
|--|--|--|
|HostAddress|String|用户连接数据库的主机地址。|
|DBName|String|数据库名称。|
|SQLText|String|查询语句。|
|QueryTimes|Long|执行时长，单位：秒。|
|LockTimes|Long|锁定时长，单位：秒。|
|ParseRowCounts|Long|解析行数。|
|ReturnRowCounts|Long|返回行数。|
|ExecutionStartTime|String|执行开始时间；格式：`YYYY-MM-DDTHH:mm:ss Z`，如2011-06-11T15:00:08Z。|

## 请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeSlowLogRecords
&DBInstanceId=gp-xxxxxxx
&StartTime=2018-07-09T08:00Z
&EndTime=2018-07-09T09:00Z
&<公共请求参数>
```

## 返回示例

**XML格式**

```
<DescribeSlowLogRecordsResponse> 
	<RequestId>542BB8D6-4268-45CC-A557-B03EFD7AB30A</RequestId>
	<Engine>gpdb</Engine>
	<PageNumber>1</PageNumber>
	<PageRecordCount>1</PageRecordCount>
	<TotalRecordCount>1</TotalRecordCount>
	<Items>
		<SQLSlowRecord>
			<HostAddress>127.0.0.1</HostAddress>
			<DBName>test</DBName>
			<SQLText> update test.zxb set id=0 limit 1</SQLText>
			<QueryTimes>123</QueryTimes>
			<LockTimes>12</LockTimes>
			<ParseRowCounts>125</ParseRowCounts>
			<ReturnRowCounts>1</ReturnRowCounts>
			<ExecutionStartTime>2018-07-09T09:00:08Z</ExecutionStartTime>
		</SQLSlowRecord>
	</Items>
</DescribeSlowLogRecordsResponse>
```

**JSON格式**

```
{
    "RequestId":"542BB8D6-4268-45CC-A557-B03EFD7AB30A",
    "Engine":"gpdb",
    "PageNumber":1,
    "PageRecordCount":1,
    "TotalRecordCount":1,
    "Items":{
        "SQLSlowRecord":[
            {
                "HostAddress":"127.0.0.1",
                "DBName":"test",
                "SQLText":" update test.zxb set id=0 limit 1",
                "QueryTimes":"123",
                "LockTimes":"12",
                "ParseRowCounts":"125",
                "ReturnRowCounts":"1",
                "ExecutionStartTime":"2018-07-09T09:00:08Z"
            }
        ]
    }
}
```

