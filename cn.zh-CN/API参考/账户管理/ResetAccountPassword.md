# ResetAccountPassword {#concept_m3l_ncm_q2b .concept}

## 描述 { .section}

该接口用于重置账户密码。必须满足以下条件，否则将操作失败：

-   当前实例状态：运行中。
-   没有被锁定。

## 请求参数 { .section}

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](cn.zh-CN/API参考/公共参数.md#)。|
|Action|String|是|系统规定参数，取值为ResetAccountPassword。|
|DBInstanceId|String|是|实例名。|
|AccountName|String|是|账户名。|
|AccountPassword|String|是|新密码，由大写字母、小写字母、数字、或者特殊字符其中的至少三种字符组成，长度为8－32位；特殊字符为!@\#$%^&\*\(\)\_+-=。|

## 返回参数 { .section}

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>| |详见[公共返回参数](cn.zh-CN/API参考/公共参数.md#section_apd_1rv_3bb)。|

## 请求示例 { .section}

```
https://gpdb.aliyuncs.com/?Action=ResetAccountPassword
&AccountName=testaccount_1
&AccountPassword=Testaccount_1
&DBInstanceId=gp-xxxxxxx
&<公共请求参数>
```

## 返回示例 { .section}

**XML格式**

```
<ResetAccountPasswordResponse>
         <RequestId>187C80FC-75C4-477C-BBF2-A368A36D041C</RequestId>
</ResetAccountPasswordResponse>
```

**JSON格式**

```
{
    "RequestId":"187C80FC-75C4-477C-BBF2-A368A36D041C"
}
```

