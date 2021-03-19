# DescribeDBInstanceSecurityIps

调取DescribeDBInstanceSecurityIps接口查询所有白名单。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceSecurityIps&type=RPC&version=2019-06-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeDBInstanceSecurityIps|系统规定参数。取值：DescribeDBInstanceSecurityIps。 |
|InstanceId|String|是|gp-xxxxxxxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|"200"|内部返回码。 |
|Count|Long|2|安全组数量。 |
|HttpStatusCode|Integer|200|通用http返回码。 |
|Message|String|""|错误详细信息。 |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|请求ID。 |
|Result|Array of Result| |结果列表。 |
|GroupName|String|default|白名单分组名。 |
|WhiteList|List|\[ "127.0.0.1"\]|白名IP单列表。 |
|Success|Boolean|true|是否成功。 |

## 示例

请求示例

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeDBInstanceSecurityIps
&InstanceId=gp-xxxxxxxxxxxx
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<Message>""</Message>
<RequestId>B4CAF581-2AC7-41AD-8940-D56DF7AADF5B</RequestId>
<HttpStatusCode>200</HttpStatusCode>
<Count>2</Count>
<Code>"200"</Code>
<Success>true</Success>
<Result>
    <GroupName>default</GroupName>
    <WhiteList> [ "127.0.0.1"]</WhiteList>
</Result>
```

`JSON`格式

```
{"Message":"\"\"","RequestId":"B4CAF581-2AC7-41AD-8940-D56DF7AADF5B","HttpStatusCode":"200","Count":"2","Code":"\"200\"","Success":"true","Result":[{"GroupName":"default","WhiteList":" [ \"127.0.0.1\"]"}]}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

