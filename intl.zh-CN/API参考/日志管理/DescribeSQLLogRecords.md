# DescribeSQLLogRecords

## 描述

查询实例的SQL审计日志。

## 请求参数

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](/intl.zh-CN/API参考/公共参数.md)。|
|Action|String|是|系统规定参数，取值：DescribeSQLLogRecords。|
|DBInstanceId|String|是|实例名。|
|Database|String|否|默认为所有数据库。|
|User|String|否|默认为所有用户。|
|QueryKeywords|String|否|关键字查询，多个关键字以空格分隔，不超过10个关键字。|
|StartTime|String|是|查询开始时间，格式如：`yyyy-MM-ddTHH:mm:ssZ`。|
|EndTime|String|是|查询结束时间，格式如：`yyyy-MM-ddTHH:mm:ssZ`，且大于查询开始时间。|
|PageSize|Integer|否|每页记录数，取值：30/50/100；默认值：30。|
|PageNumber|Integer|否|页码，大于0且不超过Integer的最大值；默认值：1。|

## 返回参数

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>| |详见[公共返回参数](/intl.zh-CN/API参考/公共参数.mdsection_apd_1rv_3bb)。|
|TotalRecordCount|Integer|总记录数。|
|PageNumber|Integer|页码。|
|PageRecordCount|Integer|本页SQL日志记录个数。|
|Items|List<SQLRecord\>|无|

|名称|类型|描述|
|--|--|--|
|DBName|String|数据库。|
|AccountName|String|账号名。|
|HostAddress|String|客户端IP地址。|
|SQLText|String|SQL语句。|
|TotalExecutionTimes|Long|消耗时间，单位：微秒。|
|ReturnRowCounts|Long|返回记录数。|
|ThreadID|String|线程ID。|
|ExecuteTime|String|执行时间;格式：`yyyy-MM-ddTHH:mm:ssZ`，如2011-05-30 T12:11:20Z。|

## 请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeSQLLogRecords
&DBInstanceId=gp-xxxxxxx
&StartTime=2018-07-09T04:50:20Z
&EndTime=2018-07-09T08:50:20Z
&<公共请求参数>
```

## 返回示例

**XML格式**

```
<DescribeSQLLogRecordsResponse> 
	<Items></Items>
	<TotalRecordCount>0</TotalRecordCount>
	<PageNumber>1</PageNumber>
	<RequestId>9B24F29F-BA7F-47A3-838E-C6A993888388</RequestId>
	<PageRecordCount>0</PageRecordCount>
</DescribeSQLLogRecordsResponse>
```

**JSON格式**

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

