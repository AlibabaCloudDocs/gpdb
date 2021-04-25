# DescribeSQLLogs

You can call this operation to query SQL execution records.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeSQLLogs&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeSQLLogs|The operation that you want to perform. Set the value to DescribeSQLLogs. |
|DBInstanceId|String|Yes|gp-xxxxxxxx|The ID of the instance. |
|EndTime|String|Yes|2021-03-17T06:30Z|The end of the time range to query. |
|StartTime|String|Yes|2021-03-10T06:30Z|The beginning of the time range to query. |
|QueryKeywords|String|No|select 1|The keywords used to query. |
|Database|String|No|adbpgadmin|The name of the database. |
|User|String|No|testadmin|The username that is used to connect to the database. |
|PageSize|Integer|No|1|The number of the page to return. |
|PageNumber|Integer|No|10|The number of entries to return on each page. |
|ExecuteCost|String|No|1|The execution duration of the query. |
|SourceIP|String|No|100.104.205.90|The source IP address. |
|ExecuteState|String|No|success|The execution status of the query. |
|OperationClass|String|No|DQL|The category of the SQL statement. |
|OperationType|String|No|SELECT|The type of the SQL statement. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Items|Array of Item| |Details about the query results. |
|AccountName|String|testadmin|The username that is used to connect to the database. |
|DBName|String|adbpgadmin|The name of the database. |
|DBRole|String|master|The role of the database. |
|ExecuteCost|Float|0.0|The execution duration of the query. |
|ExecuteState|String|success|The execution status of the query. |
|OperationClass|String|DQL|The category of the SQL statement. |
|OperationExecuteTime|String|2021-03-15T17:02:32Z|The execution time of the SQL statement. |
|OperationType|String|SELECT|The type of the SQL statement. |
|ReturnRowCounts|Long|1|The number of entries returned. |
|SQLPlan|String|""|The SQL execution plan. |
|SQLText|String|select 1|The SQL statement. |
|ScanRowCounts|Long|1|The number of scanned rows. |
|SourceIP|String|100.104.205.90|The source IP address. |
|SourcePort|Integer|50514|The number of the source port. |
|PageNumber|Integer|1|The page number of the returned page. |
|PageRecordCount|Integer|1|The number of entries returned per page. |
|RequestId|String|A7941C94-B92F-46A0-BD3E-2D0DF9A7C984|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=DescribeSQLLogs
&DBInstanceId=gp-xxxxxxxx
&EndTime=2021-03-17T06:30Z
&StartTime=2021-03-10T06:30Z
&<Common request parameters>
```

Sample success responses

`XML` format

```
<code>200</code>
<data>
    <PageRecordCount>206</PageRecordCount>
    <RequestId>A7941C94-B92F-46A0-BD3E-2D0DF9A7C984</RequestId>
    <PageNumber>1</PageNumber>
    <Items>
        <ExecuteCost>0.0</ExecuteCost>
        <SQLPlan/>
        <DBRole>master</DBRole>
        <SourcePort>50514</SourcePort>
        <SQLText> select 1</SQLText>
        <SourceIP>100.104.205.90</SourceIP>
        <ReturnRowCounts>1</ReturnRowCounts>
        <DBName>adbpgadmin</DBName>
        <OperationType>SELECT</OperationType>
        <ScanRowCounts>0</ScanRowCounts>
        <AccountName>testadmin</AccountName>
        <OperationExecuteTime>2021-03-15T17:02:32Z</OperationExecuteTime>
        <ExecuteState>success</ExecuteState>
        <OperationClass>DQL</OperationClass>
    </Items>
</data>
<httpStatusCode>200</httpStatusCode>
<requestId>A7941C94-B92F-46A0-BD3E-2D0DF9A7C984</requestId>
<successResponse>true</successResponse>
```

`JSON` format

```
{
	"code": "200",
	"data": {
		"PageRecordCount": 1,
		"RequestId": "A7941C94-B92F-46A0-BD3E-2D0DF9A7C984",
		"PageNumber": 1,
		"Items": [{
			"ExecuteCost": "0.0",
			"SQLPlan": "",
			"DBRole": "master",
			"SourcePort": 50514,
			"SQLText": " select 1",
			"SourceIP": "100.104.205.90",
			"ReturnRowCounts": 1,
			"DBName": "adbpgadmin",
			"OperationType": "SELECT",
			"ScanRowCounts": 0,
			"AccountName": "testadmin",
			"OperationExecuteTime": "2021-03-15T17:02:32Z",
			"ExecuteState": "success",
			"OperationClass": "DQL"
		}]
	},
	"httpStatusCode": "200",
	"requestId": "A7941C94-B92F-46A0-BD3E-2D0DF9A7C984",
	"successResponse": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

