# DescribeUserEncryptionKeyList

调取DescribeUserEncryptionKeyList接口获取用户开通的KMS密钥列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeUserEncryptionKeyList&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeUserEncryptionKeyList|系统规定参数。取值：DescribeUserEncryptionKeyList。 |
|RegionId|String|是|ap-southeast-1|地域信息。 |
|PageNumber|String|否|1|查询KMS页数，默认值为1。 |
|PageSize|String|否|10|每页返回KMS个数，默认值为10。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|KmsKeys|Array of KmsKeys| |KMS的key列表。 |
|KeyId|String|0b8b1825-fd99-418f-875e-e4dec1dd8715|KMS的ID。 |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|请求ID。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeUserEncryptionKeyList
&RegionId=ap-southeast-1
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<RequestId>B4CAF581-2AC7-41AD-8940-D56DF7AADF5B</RequestId>
<KmsKeys>
    <KeyId>0b8b1825-fd99-418f-875e-e4dec1dd8715</KeyId>
</KmsKeys>
```

`JSON`格式

```
{"RequestId":"B4CAF581-2AC7-41AD-8940-D56DF7AADF5B","KmsKeys":[{"KeyId":"0b8b1825-fd99-418f-875e-e4dec1dd8715"}]}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

