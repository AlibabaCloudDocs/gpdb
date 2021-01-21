# DeleteDatabase

调用DeleteDatabase删除实例下的某个数据库。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DeleteDatabase&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DeleteDatabase|系统规定参数。取值：DeleteDatabase。 |
|DBInstanceId|String|是|gp-xxxxxxxxxxx|实例ID。 |
|DBName|String|否|testdb01|数据库名。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|07F6177E-6DE4-408A-BB4F-0723301340F3|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DeleteDatabase
&DBName=testdb01
&DBInstanceId=gp-xxxxxxxxxxx
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DeleteDatabaseResponse>
       <RequestId>07F6177E-6DE4-408A-BB4F-0723301340F3</RequestId>
</DeleteDatabaseResponse>
```

`JSON` 格式

```
{
    "RequestId":"07F6177E-6DE4-408A-BB4F-0723301340F3"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

