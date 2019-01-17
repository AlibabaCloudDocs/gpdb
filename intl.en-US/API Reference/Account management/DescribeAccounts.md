# DescribeAccounts {#concept_f12_hcm_q2b .concept}

## Description { .section}

You can call this operation to query the list of accounts.

## Request parameters { .section}

|Name|Type|Required| Description|
|----|----|--------|------------|
|<Common request parameters\>|-|Yes|For more information, see [Common parameters](reseller.en-US/API Reference/Common parameters.md#).|
|Action|String|Yes|The operation that you want to perform. Set the value to DescribeAccounts.|
|DBInstanceId|String|Yes|The instance ID.|
|AccountName| String| No|The account name.|

## Response parameters { .section}

|Name |Type|Description|
|-----|----|-----------|
|<Common response paramters\>|-|For more information, see [Common response parameters](reseller.en-US/API Reference/Common parameters.md#section_apd_1rv_3bb).|
|Accounts|List<DBInstanceAccount\>|The list of accounts.|

|Name|Type|Description|
|----|----|-----------|
|DBInstanceId|String |The instance name.|
|AccountName|String|The account name.|
|AccountStatus|String |The account status is as follows:-   0: indicates that the account is being created.
-   1: indicates that the account is being used.
-   3: indicates that the account is being deleted.

|
|AccountDescription|String|The description of the account.|

## Sample requests { .section}

```
https://gpdb.aliyuncs.com/?Action=DescribeAccounts
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>
```

## Sample responses { .section}

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
          "DBInstanceId": "gp-xxxxxxx", 
          "AccountStatus": "1", 
          "AccountName": "testaccount_1"
        }
      ]
    }, 
    "RequestId": "7565770E-7C45-462D-BA4A-8A5396F2CAD1"
  }
```

