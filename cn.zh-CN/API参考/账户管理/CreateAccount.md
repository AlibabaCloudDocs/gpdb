# CreateAccount

## 描述

创建账户。

## 输入参数

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](/cn.zh-CN/API参考/公共参数.md)。|
|Action|String|是|系统规定参数，取值：CreateAccount|
|DBInstanceId|String|是|实例名|
|DatabaseName|String|否|数据库名|
|AccountName|String|是|账户名|
|AccountPassword|String|是|账户密码|
|AccountDescription|String|否|账户描述|

## 返回参数

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>|-|详见[公共返回参数](/cn.zh-CN/API参考/公共参数.mdsection_apd_1rv_3bb)。|

## 请求示例

```
https://gpdb.aliyuncs.com/?Action=CreateAccount
&AccountName=testacc02
&AccountPassword=pw1234
&DBInstanceId=gp-xxxxxxx
&<公共请求参数>
```

## 返回示例

**XML格式**

```
<CreateAccountResponse>
         <RequestId>D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD</RequestId>
</CreateAccountResponse>
```

**JSON格式**

```
{
  "RequestId":"D4D4BE8A-DD46-440A-BFCD-EE31DA81C9DD"
}
```

