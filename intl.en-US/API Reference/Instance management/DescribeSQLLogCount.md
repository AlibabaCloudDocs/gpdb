# DescribeSQLLogCount

You can call this operation to query the number of audit logs.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeSQLLogCount&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeSQLLogCount|The operation that you want to perform. Set the value to DescribeSQLLogCount. |
|DBInstanceId|String|Yes|gp-xxx|The ID of the instance. |
|EndTime|String|Yes|2020-12-12T11:22:33Z|The end of the time range to query. |
|StartTime|String|Yes|2020-12-14T11:22:33Z|The beginning of the time range to query. |
|QueryKeywords|String|No|test|The keywords used to query. |
|Database|String|No|testdb|The name of the database whose SQL audit logs you want to query. |
|User|String|No|adbpgadmin|The account that is used to log on to the database. |
|ExecuteCost|String|No|100|The execution duration of the query. |
|SourceIP|String|No|10.11.12.13|The source IP address. |
|ExecuteState|String|No|success|The execution status of the query. |
|OperationClass|String|No|DML|The category of the SQL statement. |
|OperationType|String|No|Select|The type of the SQL statement. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|DBClusterId|String|gp-xxx|The ID of the instance. |
|EndTime|String|2020-12-14T11:22:33Z|The end time of the query. |
|Items|Array of Item| |Details about the query items. |
|Name|String|count|The name of the query item. |
|Series|Array of SeriesItem| |Details about the values of the query items. |
|Values|Array of ValueItem| |Details about the value sequence of the query items. |
|Point|List|2020-12-14T11:22:33Z,100|The value of the query item. |
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|The ID of the request. |
|StartTime|String|2020-12-12T11:22:33Z|The start time of the query. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeSQLLogCount
&DBInstanceId=gp-xxx
&EndTime=2020-12-12T11:22:33Z
&StartTime=2020-12-14T11:22:33Z
&<Common request parameters>
```

Sample success responses

`XML` format

```
<EndTime>2020-12-14T11:22:33Z</EndTime>
<RequestId>7565770E-7C45-462D-BA4A-8A5396F2CAD1</RequestId>
<StartTime>2020-12-12T11:22:33Z</StartTime>
<DBClusterId>gp-xxx</DBClusterId>
<Items>
    <Name>count</Name>
    <Series>
        <Values>
            <Point>2020-12-14T11:22:33Z,100</Point>
        </Values>
    </Series>
</Items>
```

`JSON` format

```
{"EndTime":"2020-12-14T11:22:33Z","RequestId":"7565770E-7C45-462D-BA4A-8A5396F2CAD1","StartTime":"2020-12-12T11:22:33Z","DBClusterId":"gp-xxx","Items":[{"Name":"count","Series":[{"Values":[{"Point":"2020-12-14T11:22:33Z,100"}]}]}]}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

