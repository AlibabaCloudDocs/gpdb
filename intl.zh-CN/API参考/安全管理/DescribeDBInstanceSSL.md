# DescribeDBInstanceSSL

调取DescribeDBInstanceSSL接口获取SSL信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceSSL&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeDBInstanceSSL|系统规定参数。取值：DescribeDBInstanceSSL。 |
|DBInstanceId|String|是|gp-xxxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|CertCommonName|String|\*.gpdbmaster.xxx.rds.aliyuncs.com|证书信息。 |
|DBInstanceId|String|gp-xxxxx|实例ID. |
|DBInstanceName|String|gp-xxxxx|实例名。 |
|RequestId|String|D5FF8636-37F6-4CE0-8002-F8734C62C686|请求ID。 |
|SSLEnabled|Boolean|true|是否开启SSL。 |
|SSLExpiredTime|String|2020-08-05T09:05:53Z|SSL证书的过期时间。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeDBInstanceSSL
&<公共请求参数>
&DBInstanceId=gp-xxxxxx
```

正常返回示例

`XML`格式

```
<SSLEnabled>true</SSLEnabled>
<RequestId>D5FF8636-37F6-4CE0-8002-F8734C62C686</RequestId>
<DBInstanceName>gp-xxx</DBInstanceName>
```

`JSON`格式

```
{
  "SSLEnabled": true,
  "RequestId": "D5FF8636-37F6-4CE0-8002-F8734C62C686",
  "DBInstanceName": "gp-xxx"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

