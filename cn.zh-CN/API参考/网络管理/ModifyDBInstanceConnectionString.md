# ModifyDBInstanceConnectionString {#concept_ryv_khm_q2b .concept}

## 描述 { .section}

该接口用于修改实例的内外网连接地址和端口。

## 请求参数 { .section}

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](intl.zh-CN/API参考/公共参数.md#)。|
|Action|String|是|系统规定参数，取值为ModifyDBInstanceConnectionString。|
|DBInstanceId|String|是|实例名。|
|CurrentConnectionString|String|是|实例当前的某个连接地址。|
|ConnectionStringPrefix|String|是|目标连接地址。|
|Port|String|否|目标端口。|

## 返回参数 { .section}

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>|-|详见[公共返回参数](ZH-CN_TP_16898_V1.dita#reference_zpm_4wl_q2b/section_apd_1rv_3bb)。|

## 请求示例 { .section}

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceConnectionString
&DBInstanceId=gp-xxxxxxx
&CurrentConnectionString=gp-xxxxxxx.gpdb.rds.aliyuncs.com
&ConnectionStringPrefix=gp-xxxxxxx
&<公共请求参数>
```

## 返回示例 { .section}

**XML格式**

```
<ModifyDBInstanceConnectionStringResponse>
         <RequestId>29B0BF34-D069-4495-92C7-FA6D94528A9E</RequestId>
</ModifyDBInstanceConnectionStringResponse>
```

**JSON格式**

```
{
      "RequestId":"29B0BF34-D069-4495-92C7-FA6D94528A9E"
  }
```

