# ST\_SetAKId

设置raster对象OSS的AccessKey ID。

## 前提条件

raster对象必须存储在OSS。

## 语法

```
raster ST_SetAKId(raster raster_obj, text id, bool valid default true)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|id|登录OSS的AccessKey ID，如果为NULL，表示使用之前的登录ID。|
|valid|是否验证登录信息有效性。|

## 示例

```
UPDATE raster_table
SET rast = ST_SetAKId(rast, 'OSS_ACCESSKEY_ID');

SELECT ST_AKId(rast) from raster_table;
     st_akid      
------------------
 OSS_ACCESSKEY_ID
```

