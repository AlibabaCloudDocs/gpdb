# DescribeSQLLogs

调取DescribeSQLLogs接口查询SQL执行记录。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeSQLLogs&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeSQLLogs|系统规定参数。取值：DescribeSQLLogs。 |
|DBInstanceId|String|是|gp-xxxxxxxx|实例ID。 |
|EndTime|String|是|2021-03-17T06:30Z|结束时间。 |
|StartTime|String|是|2021-03-10T06:30Z|开始时间。 |
|QueryKeywords|String|否|select 1|关键字。 |
|Database|String|否|adbpgadmin|数据库名。 |
|User|String|否|testadmin|用户名。 |
|PageSize|Integer|否|1|返回的页数。 |
|PageNumber|Integer|否|10|每页记录条数。 |
|ExecuteCost|String|否|1|执行耗时。 |
|SourceIP|String|否|100.104.205.90|来源IP。 |
|ExecuteState|String|否|success|执行状态。 |
|OperationClass|String|否|DQL|操作类别。 |
|OperationType|String|否|SELECT|执行SQL的类型。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Items|Array of Item| |结果列表。 |
|AccountName|String|testadmin|账户名。 |
|DBName|String|adbpgadmin|数据库名。 |
|DBRole|String|master|数据库角色。 |
|ExecuteCost|Float|0.0|执行耗时。 |
|ExecuteState|String|success|执行状态。 |
|OperationClass|String|DQL|操作类别。 |
|OperationExecuteTime|String|2021-03-15T17:02:32Z|执行时间。 |
|OperationType|String|SELECT|执行SQL的类型。 |
|ReturnRowCounts|Long|1|执行结果返回的行数。 |
|SQLPlan|String|""|SQL执行计划。 |
|SQLText|String|select 1|SQL语句内容。 |
|ScanRowCounts|Long|1|扫描行数。 |
|SourceIP|String|100.104.205.90|来源IP。 |
|SourcePort|Integer|50514|来源端口。 |
|PageNumber|Integer|1|查询结果页数。 |
|PageRecordCount|Integer|1|每页包含的记录个数。 |
|RequestId|String|A7941C94-B92F-46A0-BD3E-2D0DF9A7C984|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeSQLLogs
&DBInstanceId=gp-xxxxxxxx
&EndTime=2021-03-17T06:30Z
&StartTime=2021-03-10T06:30Z
&<公共请求参数>
```

正常返回示例

`XML`格式

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

`JSON`格式

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

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

