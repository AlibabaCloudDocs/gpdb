# CreateAccount

## Description

Creates an account for an instance.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to CreateAccount.|
|DBInstanceId|String|Yes|The ID of the instance.|
|DatabaseName|String|No|The name of the database.|
|AccountName|String|Yes|The username of the account.|
|AccountPassword|String|Yes|The password of the account.|
|AccountDescription|String|No|The description of the account.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=CreateAccount
&AccountName=testacc02
&AccountPassword=pw1234
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>
```

## Sample responses

**XML format**

```
<CreateAccountResponse>
         <RequestId>D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD</RequestId>
</CreateAccountResponse>
```

**JSON format**

```
{
  "RequestId":"D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD"
}
```

