# DescribeSQLLogCount

调取DescribeSQLLogCount接口获取审计日志数量。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeSQLLogCount&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeSQLLogCount|系统规定参数。取值：DescribeSQLLogCount。 |
|DBInstanceId|String|是|gp-xxx|实例ID。 |
|EndTime|String|是|2020-12-12T11:22:33Z|查询结束时间。 |
|StartTime|String|是|2020-12-14T11:22:33Z|查询开始时间。 |
|QueryKeywords|String|否|test|查询关键词。 |
|Database|String|否|testdb|数据库名称。 |
|User|String|否|adbpgadmin|账号。 |
|ExecuteCost|String|否|100|执行代价。 |
|SourceIP|String|否|10.11.12.13|源IP。 |
|ExecuteState|String|否|success|执行状态。 |
|OperationClass|String|否|DML|SQL类别。 |
|OperationType|String|否|Select|SQL类型。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|DBClusterId|String|gp-xxx|实例ID。 |
|EndTime|String|2020-12-14T11:22:33Z|查询结束时间。 |
|Items|Array of Item| |查询项。 |
|Name|String|count|名称。 |
|Series|Array of SeriesItem| |系列值。 |
|Values|Array of ValueItem| |系列值。 |
|Point|List|2020-12-14T11:22:33Z,100|查询值。 |
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|请求ID。 |
|StartTime|String|2020-12-12T11:22:33Z|查询开始时间。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeSQLLogCount
&DBInstanceId=gp-xxx
&EndTime=2020-12-12T11:22:33Z
&StartTime=2020-12-14T11:22:33Z
&<公共请求参数>
```

正常返回示例

`XML`格式

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

`JSON`格式

```
{"EndTime":"2020-12-14T11:22:33Z","RequestId":"7565770E-7C45-462D-BA4A-8A5396F2CAD1","StartTime":"2020-12-12T11:22:33Z","DBClusterId":"gp-xxx","Items":[{"Name":"count","Series":[{"Values":[{"Point":"2020-12-14T11:22:33Z,100"}]}]}]}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

