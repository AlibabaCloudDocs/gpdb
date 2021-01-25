# DescribeAccounts

## Description

Queries the accounts created on an AnalyticDB for PostgreSQL instance.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to DescribeAccounts.|
|DBInstanceId|String|Yes|The ID of the instance.|
|AccountName|String|No|The name of the initial user created during instance creation.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>|N/A|For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|
|Accounts|List<DBInstanceAccount\>|An array consisting of accounts.|

|Field|Type|Description|
|-----|----|-----------|
|DBInstanceId|String|The ID of the instance.|
|AccountName|String|The name of the account.|
|AccountStatus|String|The status of the account. Valid values: -   0: The account is being created.
-   1: The account is in use.
-   3: The account is being deleted. |
|AccountDescription|String|The description of the account.|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeAccounts
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>
```

## Sample responses

**XML format**

```
<DescribeAccountsResponse>
         <RequestId>7565770E-7C45-462D-BA4A-8A5396F2CAD1</RequestId>
           <Accounts>
               <DBInstanceAccount>
                  <AccountName>testaccount_1</AccountName>
                  <DBInstanceId>gp-xxxxxxx</DBInstanceId>
                  <AccountStatus>1</AccountStatus>
                  <AccountDescription></AccountDescription>
               <DBInstanceAccount>
           </Accounts>
</DescribeAccountsResponse>
```

**JSON format**

```
{
    "Accounts": {
      "DBInstanceAccount": [
        {
          "AccountDescription": "", 
          "DBInstanceId":"pgm-xxxxxxx", 
          "AccountStatus": "1", 
          "AccountName": "testaccount_1"
        }
      ]
    }, 
    "RequestId": "7565770E-7C45-462D-BA4A-8A5396F2CAD1"
  }
```

