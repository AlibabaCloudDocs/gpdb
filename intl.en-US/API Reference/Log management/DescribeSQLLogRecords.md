# DescribeSQLLogRecords

## Description

Queries the SQL audit logs generated for an instance over a specific time range.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to DescribeSQLLogRecords.|
|DBInstanceId|String|Yes|The ID of the instance.|
|Database|String|No|The database whose SQL audit logs you want to query. By default, the SQL audit logs of all databases in the instance are queried.|
|User|String|No|The account to which the database belongs. By default, the SQL audit logs of databases under all accounts are queried.|
|QueryKeywords|String|No|The keywords used for query. You can enter up to 10 keywords at a time but must separate them with spaces.|
|StartTime|String|Yes|The beginning of the time range to query. Specify the time in the `yyyy-MM-ddTHH:mm:ssZ` format.|
|EndTime|String|Yes|The end of the time range to query. Specify the time in the `yyyy-MM-ddTHH:mm:ssZ` format. The end time must be later than the start time.|
|PageSize|Integer|No|The number of entries to return on each page. Valid values: 30, 50, and 100. Default value: 30.|
|PageNumber|Integer|No|The number of the page to return. Valid values: any non-zero positive integer. Default value: 1.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>| |For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|
|TotalRecordCount|Integer|The total number of entries.|
|PageNumber|Integer|The number of the returned page.|
|PageRecordCount|Integer|The number of SQL audit log entries returned on the current page.|
|Items|List<SQLRecord\>|An array consisting of SQLRecord data.|

|Parameter|Type|Description|
|---------|----|-----------|
|DBName|String|The name of the database.|
|AccountName|String|The name of the account.|
|HostAddress|String|The IP address of the client.|
|SQLText|String|The SQL statement.|
|TotalExecutionTimes|Long|The amount of time taken to execute the statement. Unit: microseconds.|
|ReturnRowCounts|Long|The number of entries returned.|
|ThreadID|String|The ID of the thread.|
|ExecuteTime|String|The time when the query was executed. The time is in the `yyyy-MM-ddTHH:mm:ssZ` format, such as 2011-05-30 T12:11:20Z.|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeSQLLogRecords
&DBInstanceId=gp-xxxxxxx
&StartTime=2018-07-09T04:50:20Z
&EndTime=2018-07-09T08:50:20Z
&<Common request parameters>
```

## Sample responses

**XML format**

```
<DescribeSQLLogRecordsResponse> 
    <Items></Items>
    <TotalRecordCount>0</TotalRecordCount>
    <PageNumber>1</PageNumber>
    <RequestId>9B24F29F-BA7F-47A3-838E-C6A993888388</RequestId>
    <PageRecordCount>0</PageRecordCount>
</DescribeSQLLogRecordsResponse>
```

**JSON format**

```
{
    "Items":{
        "SQLRecord":[
        ]
    },
    "TotalRecordCount":0,
    "PageNumber":1,
    "RequestId":"9B24F29F-BA7F-47A3-838E-C6A993888388",
    "PageRecordCount":0
}
```

