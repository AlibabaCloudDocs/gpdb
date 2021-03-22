# DescribeSQLLogRecords

调用DescribeSQLLogRecords查询实例的SQL审计日志。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeSQLLogRecords&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeSQLLogRecords|系统规定参数。取值：DescribeSQLLogRecords。 |
|DBInstanceId|String|是|gp-xxxxxxxxxx|实例ID。 |
|EndTime|String|是|2018-07-09T08:50:20Z|查询结束时间，格式如：yyyy-MM-ddTHH:mm:ssZ，且大于查询开始时间。 |
|StartTime|String|是|2018-07-09T04:50:20Z|查询开始时间，格式如：yyyy-MM-ddTHH:mm:ssZ。 |
|QueryKeywords|String|否|keywords1|关键字查询，多个关键字以空格分隔，不超过10个关键字。 |
|Database|String|否|testdb|数据库名，默认为所有数据库。 |
|User|String|否|testacc|用户名，默认为所有用户。 |
|Form|String|否|Stream|触发审计文件的生成或者返回SQL记录列表。 |
|PageSize|Integer|否|30|每页记录数，取值：30/50/100；默认值：30。 |
|PageNumber|Integer|否|1|页码，大于0且不超过Integer的最大值；默认值：1。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Items|Array of SQLRecord| |SQL记录列表。 |
|SQLRecord| | | |
|AccountName|String|testacc|账号名。 |
|DBName|String|testdb|数据库。 |
|ExecuteTime|String|2018-07-09T08:50:20Z|执行时间;格式：`yyyy-MM-ddTHH:mm:ssZ`，如2011-05-30 T12:11:20Z。 |
|HostAddress|String|127.0.0.1|客户端IP地址。 |
|ReturnRowCounts|Long|122|返回记录数。 |
|SQLText|String|update test.zxb set id=0 limit 1|SQL语句。 |
|ThreadID|String|723|线程ID。 |
|TotalExecutionTimes|Long|130|消耗时间，单位：微秒。 |
|PageNumber|Integer|1|页码。 |
|PageRecordCount|Integer|1|本页SQL日志记录个数。 |
|RequestId|String|9B24F29F-BA7F-47A3-838E-C6A993888388|请求ID。 |
|TotalRecordCount|Integer|1|总记录数。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeSQLLogRecords
&DBInstanceId=gp-xxxxxxxxxx
&EndTime=2018-07-09T08:50:20Z
&StartTime=2018-07-09T04:50:20Z
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<DescribeSQLLogRecords>
  <Items>
        <SQLRecord>
              <AccountName>testacc</AccountName>
              <DBName>testdb</DBName>
              <ExecuteTime>2018-07-09T08:50:20Z</ExecuteTime>
              <HostAddress>127.0.0.1</HostAddress>
              <ReturnRowCounts>122</ReturnRowCounts>
              <SQLText>update test.zxb set id=0 limit 1</SQLText>
              <ThreadID>723</ThreadID>
              <TotalExecutionTimes>130</TotalExecutionTimes>
        </SQLRecord>
  </Items>
  <TotalRecordCount>1</TotalRecordCount>
  <PageNumber>1</PageNumber>
  <RequestId>9B24F29F-BA7F-47A3-838E-C6A993888388</RequestId>
  <PageRecordCount>1</PageRecordCount>
</DescribeSQLLogRecords>
```

`JSON`格式

```
{
	"Items": {
		"SQLRecord": [
			{
				"AccountName": "testacc",
				"DBName": "testdb",
				"ExecuteTime": "2018-07-09T08:50:20Z",
				"HostAddress": "127.0.0.1",
				"ReturnRowCounts": "122",
				"SQLText": "update test.zxb set id=0 limit 1",
				"ThreadID": "723",
				"TotalExecutionTimes": "130"
			}
		]
	},
	"TotalRecordCount": 1,
	"PageNumber": 1,
	"RequestId": "9B24F29F-BA7F-47A3-838E-C6A993888388",
	"PageRecordCount": 1
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

