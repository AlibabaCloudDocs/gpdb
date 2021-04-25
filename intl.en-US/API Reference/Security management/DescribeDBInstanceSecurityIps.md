# DescribeDBInstanceSecurityIps

You can call this operation to query the IP address whitelists of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceSecurityIps&type=RPC&version=2019-06-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeDBInstanceSecurityIps|The operation that you want to perform. Set the value to DescribeDBInstanceSecurityIps. |
|InstanceId|String|Yes|gp-xxxxxxxxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|"200"|The internal status code. |
|Count|Long|2|The number of security groups. |
|HttpStatusCode|Integer|200|The HTTP status code. |
|Message|String|""|The returned error message. |
|RequestId|String|B4CAF581-2AC7-41AD-8940-D56DF7AADF5B|The ID of the request. |
|Result|Array of Result| |Details about the IP address whitelists. |
|GroupName|String|default|The name of the IP address whitelist. |
|WhiteList|List|\[ "127.0.0.1"\]|The list of one or more IP addresses that the whitelist contains. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeDBInstanceSecurityIps
&InstanceId=gp-xxxxxxxxxxxx
&<Common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

```
{"Message":"\"\"","RequestId":"B4CAF581-2AC7-41AD-8940-D56DF7AADF5B","HttpStatusCode":"200","Count":"2","Code":"\"200\"","Success":"true","Result":[{"GroupName":"default","WhiteList":" [ \"127.0.0.1\"]"}]}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

