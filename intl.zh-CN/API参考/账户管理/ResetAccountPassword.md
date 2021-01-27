# ResetAccountPassword

调用ResetAccountPassword重置账户密码。

必须满足以下条件，否则将操作失败：

-   当前实例状态：运行中。
-   没有被锁定。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=ResetAccountPassword&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|AccountName|String|是|testaccount\_1|账户名。 |
|AccountPassword|String|是|Testaccount\_1|新密码，由大写字母、小写字母、数字、或者特殊字符其中的至少三种字符组成，长度为8－32位；特殊字符为`!@#$%^&*()_+-=`。 |
|DBInstanceId|String|是|gp-xxxxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|187C80FC-75C4-477C-BBF2-A368A36D041C|请求ID。 |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=ResetAccountPassword
&AccountName=testaccount_1
&AccountPassword=Testaccount_1
&DBInstanceId=gp-xxxxxxxxx
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<ResetAccountPasswordResponse>
           <RequestId>187C80FC-75C4-477C-BBF2-A368A36D041C</RequestId>
</ResetAccountPasswordResponse>
```

`JSON`格式

```
{
    "RequestId":"187C80FC-75C4-477C-BBF2-A368A36D041C"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

