# DescribeSQLLogByQueryId

调用DescribeSQLLogByQueryId接口获取指定慢SQL的详细信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeSQLLogByQueryId&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeSQLLogByQueryId|系统规定参数。取值：**DescribeSQLLogByQueryId**。 |
|DBInstanceId|String|是|gp-\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*|实例ID。

 **说明：** 您可以调用[DescribeDBInstances](~~86911~~)接口查看目标地域下所有的AnalyticDB PostgreSQL实例的详情，包括实例ID。 |
|QueryId|String|是|111111|查询ID。

 **说明：** 您可以调用[DescribeSlowSQLLogs](~~260863~~)接口查看SQL的查询ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|07F6177E-\*\*\*\*-\*\*\*\*-\*\*\*\*-0723301340F3|请求ID。 |
|Items|Array of SQLLog| |SQL详情列表。 |
|OperationClass|String|DQL|操作类别。 |
|ExecuteState|String|success|执行状态。 |
|ExecuteCost|Float|1|SQL执行时长。 |
|SQLText|String|select \*\* from \*\*|SQL语句。 |
|SourcePort|Integer|50514|源端口号。 |
|DBRole|String|master|ROLE类别。 |
|OperationType|String|select|操作类型。 |
|SourceIP|String|192.\*\*.\*\*.121|源端IP。 |
|SQLPlan|String|\*\*\*\*|SQL查询计划。 |
|ReturnRowCounts|Long|1|返回行数。 |
|DBName|String|adbpgadmin|数据库名称。 |
|OperationExecuteTime|String|2021-03-15T17:02:32Z|执行时间。 |
|ScanRowCounts|Long|1|扫描行数。 |
|AccountName|String|testadmin|数据库账号。 |
|QueryId|String|111111|查询ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeSQLLogByQueryId
&DBInstanceId=gp-****************
&QueryId=111111
&公共请求参数
```

正常返回示例

`XML`格式

```
HTTP/1.1 200 OK
Content-Type:application/xml

<DescribeSQLLogByQueryIdResponse>
    <RequestId>07F6177E-****-****-****-0723301340F3</RequestId>
    <Items>
        <OperationClass>DQL</OperationClass>
        <ExecuteState>success</ExecuteState>
        <ExecuteCost>1</ExecuteCost>
        <SQLText>select ** from **</SQLText>
        <SourcePort>50514</SourcePort>
        <DBRole>master</DBRole>
        <OperationType>select</OperationType>
        <SourceIP>192.**.**.121</SourceIP>
        <SQLPlan>****</SQLPlan>
        <ReturnRowCounts>1</ReturnRowCounts>
        <DBName>adbpgadmin</DBName>
        <OperationExecuteTime>2021-03-15T17:02:32Z</OperationExecuteTime>
        <ScanRowCounts>1</ScanRowCounts>
        <AccountName>testadmin</AccountName>
        <QueryId>111111</QueryId>
    </Items>
</DescribeSQLLogByQueryIdResponse>
```

`JSON`格式

```
HTTP/1.1 200 OK
Content-Type:application/json

{
  "RequestId" : "07F6177E-****-****-****-0723301340F3",
  "Items" : [ {
    "OperationClass" : "DQL",
    "ExecuteState" : "success",
    "ExecuteCost" : 1,
    "SQLText" : "select ** from **",
    "SourcePort" : 50514,
    "DBRole" : "master",
    "OperationType" : "select",
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

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

