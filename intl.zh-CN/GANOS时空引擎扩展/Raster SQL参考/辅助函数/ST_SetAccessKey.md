# ST\_SetAccessKey

设置raster对象OSS的登录信息。

## 前提条件

raster对象必须存储在OSS。

## 语法

```
raster ST_SetAccessKey(raster raster_obj, text id, text key, bool valid default true)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|id|登录OSS的AccessKey ID，如果为NULL，表示使用之前的登录ID。|
|key|登录OSS的AccessKey Secret，如果为NULL，表示使用之前的登录Secret。|
|valid|是否验证登录信息有效性。|

## 示例

```
UPDATE raster_table
SET rast = ST_SetAccessKey(rast, 'OSS_ACCESSKEY_ID', 'OSS_ACCESSKEY_SECRET');

SELECT ST_AKId(rast) from raster_table;
     st_akid      
------------------
 OSS_ACCESSKEY_ID
```

