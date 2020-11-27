# ST\_IsGeoreferenced

获取raster对象是否已被地理参考。boolean格式的返回结果为：“t” 或 “f”。

## 语法

```
boolean ST_IsGeoreferenced(raster raster_obj)
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|

## 描述

返回结果t表示true，f表示false。

## 示例

```
select ST_IsGeoreferenced(raster_obj) from raster_table where id=1;

__________________________________
t
```

