# DescribeSlowLogRecords

调用DescribeSlowLogRecords查询某个时间段内某个用户实例的某个数据库的慢查询明细。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeSlowLogRecords&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeSlowLogRecords|系统规定参数。取值：DescribeSlowLogRecords。 |
|DBInstanceId|String|是|gp-xxxxxxx|实例ID。 |
|EndTime|String|是|2018-07-19T09:00:08Z|查询开始日期，格式：`YYYY-MM-DDTHH:mmZ`。 |
|StartTime|String|是|2018-07-09T09:00:08Z|查询开始日期，格式：`YYYY-MM-DDTHH:mmZ`。 |
|SQLId|Long|否|143242632|SQL ID。 |
|DBName|String|否|test|数据库名称。 |
|PageSize|Integer|否|30|每页记录数，取值：30/50/100；默认值：30。 |
|PageNumber|Integer|否|1|页码，大于0且不超过Integer的最大值；默认值：1。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Engine|String|gpdb|数据库类型。 |
|Items|Array of SQLSlowRecord| |由SQLSlowRecord组成的数组。 |
|SQLSlowRecord| | | |
|DBName|String|test|数据库名称。 |
|ExecutionStartTime|String|2018-07-09T09:00:08Z|执行开始时间；格式：YYYY-MM-DDTHH:mm:ss Z，如2011-06-11T15:00:08Z。 |
|HostAddress|String|127.0.0.1|用户连接数据库的主机地址。 |
|LockTimes|Long|12|锁定时长，单位：秒。 |
|ParseRowCounts|Long|125|解析行数。 |
|QueryTimes|Long|123|执行时长，单位：秒。 |
|ReturnRowCounts|Long|1|返回行数。 |
|SQLText|String|update test.zxb set id=0 limit 1|查询语句。 |
|PageNumber|Integer|1|页码。 |
|PageRecordCount|Integer|1|本页SQL语句个数。 |
|RequestId|String|542BB8D6-4268-45CC-A557-B03EFD7AB30A|请求ID。 |
|TotalRecordCount|Integer|1|总记录数。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeSlowLogRecords
&DBInstanceId=gp-xxxxxxx
&EndTime=2018-07-19T09:00:08Z
&StartTime=2018-07-09T09:00:08Z
&<公共请求参数>
```

正常返回示例

`XML`格式

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

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

