# ST\_AKId

获取raster对象OSS的AccessKey ID，可用于配合批量修改raster对象的OSS登录密钥。

## 前提条件

raster对象必须存储在OSS。

## 语法

```
text ST_AKId(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 示例

```
SELECT ST_AKId(rast) from raster_table;

     st_akid      
------------------
 OSS_ACCESSKEY_ID
```

