# DescribeSlowSQLLogs

调用DescribeSlowSQLLogs接口查询AnalyticDB PostgreSQL版实例指定时间段的慢日志。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeSlowSQLLogs&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeSlowSQLLogs|系统规定参数。取值：**DescribeSlowSQLLogs**。 |
|DBInstanceId|String|是|gp-\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*|实例ID。

 **说明：** 您可以调用[DescribeDBInstances](~~86911~~)接口查看目标地域下所有的AnalyticDB PostgreSQL实例的详情，包括实例ID。 |
|QueryKeywords|String|否|\*\*\*\*\*|对查询包含目标关键字的SQL进行查询。 |
|StartTime|String|是|2021-03-10T06:30:00Z|查询开始时间，格式为*yyyy-MM-ddTHH:mm:ssZ*（UTC时间）。 |
|Database|String|否|adbpgadmin|数据库名称。 |
|User|String|否|testadmin|数据库用户名。 |
|EndTime|String|是|2021-03-17T06:30:00Z|查询结束时间，格式为*yyyy-MM-ddTHH:mm:ssZ*（UTC时间）。 |
|PageSize|Integer|否|30|每页记录数，取值为**30**（默认值）、**50**或**100**。 |
|PageNumber|Integer|否|1|页码，取值为大于0且不超过Integer数据类型的最大值。默认值为**1**。 |
|SourceIP|String|否|192.\*\*.\*\*.121|源端IP。 |
|ExecuteState|String|否|success|执行状态。

 -   **success**：成功。
-   **fail**：失败。 |
|OperationClass|String|否|DQL|操作类别，例如DQL、DML、DDL等。 |
|OperationType|String|否|SELECT|操作类型，例如SELECT。 |
|MinExecuteCost|String|否|1|慢SQL最小耗时，单位为秒（s）。 |
|MaxExecuteCost|String|否|1000|慢SQL最大耗时，单位为秒（s）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|PageRecordCount|Integer|1|本页SQL日志记录个数。 |
|RequestId|String|07F6177E-\*\*\*\*-\*\*\*\*-\*\*\*\*-0723301340F3|请求ID。 |
|PageNumber|Integer|1|页码。 |
|Items|Array of Item| |慢日志详情列表。 |
|OperationClass|String|DQL|操作类别。 |
|ExecuteState|String|success|执行状态。 |
|ExecuteCost|Float|2|执行时长。 |
|SQLText|String|select \*\* from \*\*|SQL语句。 |
|SourcePort|Integer|50514|源端口号。 |
|DBRole|String|master|ROLE类别。 |
|OperationType|String|SELECT|操作类型。 |
|SourceIP|String|192.\*\*.\*\*.121|源端IP。 |
|SQLPlan|String|\*\*\*\*|SQL查询计划。 |
|ReturnRowCounts|Long|1|返回行数。 |
|DBName|String|adbpgadmin|数据库名称。 |
|OperationExecuteTime|String|2021-03-15T17:02:32Z|SQL开始执行的时间。 |
|ScanRowCounts|Long|1|扫描行数。 |
|AccountName|String|testadmin|数据库账号。 |
|QueryId|String|111111|查询ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeSlowSQLLogs
&DBInstanceId=gp-****************
&StartTime=2021-03-10T06:30Z
&EndTime=2021-03-17T06:30Z
&公共请求参数
```

正常返回示例

`XML`格式

```
HTTP/1.1 200 OK
Content-Type:application/xml

<DescribeSlowSQLLogsResponse>
    <PageRecordCount>1</PageRecordCount>
    <RequestId>07F6177E-****-****-****-0723301340F3</RequestId>
    <PageNumber>1</PageNumber>
    <Items>
        <OperationClass>DQL</OperationClass>
        <ExecuteState>success</ExecuteState>
        <ExecuteCost>2</ExecuteCost>
        <SQLText>select ** from **</SQLText>
        <SourcePort>50514</SourcePort>
        <DBRole>master</DBRole>
        <OperationType>SELECT</OperationType>
        <SourceIP>192.**.**.121</SourceIP>
        <SQLPlan>****</SQLPlan>
        <ReturnRowCounts>1</ReturnRowCounts>
        <DBName>adbpgadmin</DBName>
        <OperationExecuteTime>2021-03-15T17:02:32Z</OperationExecuteTime>
        <ScanRowCounts>1</ScanRowCounts>
        <AccountName>testadmin</AccountName>
        <QueryId>111111</QueryId>
    </Items>
</DescribeSlowSQLLogsResponse>
```

`JSON`格式

```
HTTP/1.1 200 OK
Content-Type:application/json

{
  "PageRecordCount" : 1,
  "RequestId" : "07F6177E-****-****-****-0723301340F3",
  "PageNumber" : 1,
  "Items" : [ {
    "OperationClass" : "DQL",
    "ExecuteState" : "success",
    "ExecuteCost" : 2,
    "SQLText" : "select ** from **",
    "SourcePort" : 50514,
    "DBRole" : "master",
    "OperationType" : "SELECT",
    "SourceIP" : "192.**.**.121",
    "SQLPlan" : "****",
    "ReturnRowCounts" : 1,
    "DBName" : "adbpgadmin",
    "OperationExecuteTime" : "2021-03-15T17:02:32Z",
    "ScanRowCounts" : 1,
    "AccountName" : "testadmin",
    "QueryId" : "111111"
  } ]
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

