# ModifyDBInstanceSecurityIps

You can call this operation to modify an IP address whitelist of an instance.

****

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceSecurityIps&type=RPC&version=2019-06-20)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifyDBInstanceSecurityIps|The operation that you want to perform. Set the value to ModifyDBInstanceSecurityIps. |
|AliUid|Long|Yes|123456789123456|The ID of the Alibaba Cloud account. |
|GroupName|String|Yes|WhileListGroupName|The name of the IP address whitelist. |
|InstanceId|String|Yes|gp-xxxxxxxxxxx|The ID of the instance. |
|WhileList|String|Yes|"127.0.0.1,192.168.0.1"|The IP address whitelist of the instance. Separate multiple IP addresses with commas \(,\). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|String|"200"|The internal status code. |
|HttpStatusCode|Integer|200|The HTTP status code. |
|Message|String|Successful|The returned message. |
|RequestId|String|D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com//?Action=ModifyDBInstanceSecurityIps
&AliUid=123456789123456
&GroupName=WhileListGroupName
&InstanceId=gp-xxxxxxxxxxx
&WhileList="127.0.0.1,192.168.0.1"
&<Common request parameters>
```

Sample success responses

`XML` format

```
<Message>Successful</Message>
<RequestId>API-83e15d2d-86a8-474f-af96-4fbb81272471</RequestId>
<HttpStatusCode>200</HttpStatusCode>
<Code>200</Code>
<Success>true</Success>
```

`JSON` format

```
{
  "Message": "Successful",
  "RequestId": "D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD",
  "HttpStatusCode": 200,
  "Code": "200",
  "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

