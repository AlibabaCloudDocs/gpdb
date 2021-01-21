# DescribeAccounts

## 描述

查询数据库账户信息。

## 输入参数

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](/cn.zh-CN/API参考/公共参数.md)。|
|Action|String|是|系统规定参数，取值：DescribeAccounts。|
|DBInstanceId|String|是|实例ID。|
|AccountName|String|否|账户名。|

## 返回参数

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>|-|详见[公共返回参数](/cn.zh-CN/API参考/公共参数.mdsection_apd_1rv_3bb)。|
|Accounts|List<DBInstanceAccount\>|由Account组成的数组。|

|名称|类型|描述|
|--|--|--|
|DBInstanceId|String|实例名。|
|AccountName|String|账户名。|
|AccountStatus|String|账户状态：-   0：创建中
-   1：使用中
-   3：删除中 |
|AccountDescription|String|账户描述。|

## 请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeAccounts
&DBInstanceId=gp-xxxxxxx
&<公共请求参数>
```

## 返回示例

**XML格式**

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

**JSON格式**

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

