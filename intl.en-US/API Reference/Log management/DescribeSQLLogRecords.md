# DescribeSQLLogRecords

Queries the SQL audit logs that are generated for an instance over a specific time range.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeSQLLogRecords&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeSQLLogRecords|The operation that you want to perform. Set the value to DescribeSQLLogRecords. |
|DBInstanceId|String|Yes|gp-xxxxxxxxxx|The ID of the instance. |
|EndTime|String|Yes|2018-07-09T08:50:20Z|The end of the time range to query. Specify the time in the yyyy-MM-ddTHH:mm:ssZ format. The end time must be later than the start time. |
|StartTime|String|Yes|2018-07-09T04:50:20Z|The beginning of the time range to query. Specify the time in the yyyy-MM-ddTHH:mm:ssZ format. |
|QueryKeywords|String|No|keywords1|The keywords that are used for queries. Separate multiple keywords with spaces. The maximum number of keywords is 10. |
|Database|String|No|testdb|The name of the database to be queried. By default, all databases are queried. |
|User|String|No|testacc|The username of the database. By default, all users are selected. |
|Form|String|No|Stream|Specifies whether to generate SQL audit log files or return SQL audit log records. |
|PageSize|Integer|No|30|The number of entries to return on each page. Valid values: 30, 50, and 100. Default value: 30. |
|PageNumber|Integer|No|1|The number of the page to return. The value must be an integer that is greater than 0. Default value: 1. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Items|Array of SQLRecord| |Details about the SQL queries. |
|SQLRecord| | | |
|AccountName|String|testacc|The account name. |
|DBName|String|testdb|The name of the database. |
|ExecuteTime|String|2018-07-09T08:50:20Z|The time when the statement is executed. The time is in the `yyyy-MM-ddTHH:mm:ssZ` format. Example: 2011-05-30 T12:11:20Z. |
|HostAddress|String|127.0.0.1|The IP address of the client. |
|ReturnRowCounts|Long|122|The number of returned entries. |
|SQLText|String|update test.zxb set id=0 limit 1|The SQL statement that is recorded in the log. |
|ThreadID|String|723|The ID of the thread. |
|TotalExecutionTimes|Long|130|The time that is consumed to execute the statement. Unit: milliseconds. |
|PageNumber|Integer|1|The page number of the returned page. |
|PageRecordCount|Integer|1|The number of SQL audit log records on the current page. |
|RequestId|String|9B24F29F-BA7F-47A3-838E-C6A993888388|The ID of the request. |
|TotalRecordCount|Integer|1|The total number of entries. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=DescribeSQLLogRecords
&DBInstanceId=gp-xxxxxxxxxx
&EndTime=2018-07-09T08:50:20Z
&StartTime=2018-07-09T04:50:20Z
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

