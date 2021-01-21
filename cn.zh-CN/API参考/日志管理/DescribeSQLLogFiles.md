# DescribeSQLLogFiles

## 描述

查询SQL审计文件列表。

## 请求参数

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](/cn.zh-CN/API参考/公共参数.md)。|
|Action|String|是|系统规定参数，取值：DescribeSQLLogFiles。|
|DBInstanceId|String|是|实例名。|
|FileName|String|否|文件名。|
|PageSize|Integer|否|每页记录数，取值：30/50/100；默认值：30。|
|PageNumber|Integer|否|页码，大于0且不超过Integer的最大值；默认值：1。|

## 返回参数

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>| |详见[公共返回参数](/cn.zh-CN/API参考/公共参数.mdsection_apd_1rv_3bb)。|
|TotalRecordCount|Interger|总记录数。|
|PageNumber|Interger|页码。|
|PageRecordCount|Interger|本页记录数。|
|Items|List<LogFile\>|由LogFile组成的数组。|

|名称|类型|描述|
|--|--|--|
|FileID|Interger|文件ID。|
|LogStatus|String|-   NoStart：归档未开始。
-   Generating：归档中。
-   Success：归档完成。
-   Failed：归档失败。 |
|LogStartTime|String|SQL起始时间。|
|LogEndTime|String|SQL结束时间。|
|LogDownloadURL|String|下载链接。若当前不可下载，则为空串。|
|LogSize|Long|日志文件大小，单位：Byte。|

## 请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeSQLLogFiles
&DBInstanceId=gp-xxxxxxx
&<公共请求参数>
```

## 返回示例

**XML格式**

```
<DescribeSQLLogFilesResponse> 
	<Items></Items>
	<PageNumber>1</PageNumber>
	<TotalRecordCount>0</TotalRecordCount>
	<RequestId>66E164C1-D7F2-4D85-849D-E2883FCFFBA5</RequestId>
	<PageRecordCount>0</PageRecordCount>
</DescribeSQLLogFilesResponse>
```

**JSON格式**

```
{
    "Items":{
        "LogFile":[

        ]
    },
    "PageNumber":1,
    "TotalRecordCount":0,
    "RequestId":"66E164C1-D7F2-4D85-849D-E2883FCFFBA5",
    "PageRecordCount":0
}

```

