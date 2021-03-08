# DescribeSlowLogRecords

You can call this operation to query slow query details of a database in an instance in a specified time range.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeSlowLogRecords&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeSlowLogRecords|The operation that you want to perform. Set the value to DescribeSlowLogRecords. |
|DBInstanceId|String|Yes|gp-xxxxxxx|The ID of the instance. |
|EndTime|String|Yes|2018-07-19T09:00:08Z|The end of the time range to query. Specify the time in the `YYYY-MM-DDTHH:mmZ` format. |
|StartTime|String|Yes|2018-07-09T09:00:08Z|The beginning of the time range to query. Specify the time in the `YYYY-MM-DDTHH:mmZ` format. |
|SQLId|Long|No|143242632|The ID of the slow query. |
|DBName|String|No|test|The name of the database. |
|PageSize|Integer|No|30|The number of entries to return on each page. Valid values: 30, 50, and 100. Default value: 30. |
|PageNumber|Integer|No|1|The number of the page to return. The value must be an integer that is larger than 0. Default value: 1. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Engine|String|gpdb|The database engine of the instance. |
|Items|Array of SQLSlowRecord| |Details about the slow queries. |
|SQLSlowRecord| | | |
|DBName|String|test|The name of the database. |
|ExecutionStartTime|String|2018-07-09T09:00:08Z|The time when the query was started. The time is in the YYYY-MM-DDTHH:mm:ssZ format. Example: 2011-06-11T15:00:08Z. |
|HostAddress|String|127.0.0.1|The IP address of the host that is used to connect to the database. |
|LockTimes|Long|12|The lock duration of the query. Unit: seconds. |
|ParseRowCounts|Long|125|The number of parsed rows. |
|QueryTimes|Long|123|The execution duration of the query. Unit: seconds. |
|ReturnRowCounts|Long|1|The number of returned rows. |
|SQLText|String|update test.zxb set id=0 limit 1|The SQL query statement. |
|PageNumber|Integer|1|The page number of the page returned. |
|PageRecordCount|Integer|1|The number of slow queries on the current page. |
|RequestId|String|542BB8D6-4268-45CC-A557-B03EFD7AB30A|The ID of the request. |
|TotalRecordCount|Integer|1|The total number of entries. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeSlowLogRecords
&DBInstanceId=gp-xxxxxxx
&EndTime=2018-07-19T09:00:08Z
&StartTime=2018-07-09T09:00:08Z
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

