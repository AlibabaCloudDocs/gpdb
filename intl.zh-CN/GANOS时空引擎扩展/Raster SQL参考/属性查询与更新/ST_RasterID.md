# ST\_RasterID

获得raster对象的UUID（通用唯一识别码）。

## 语法

```
text ST_RasterID(raster raster_obj);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|

## 示例

```
select ST_RasterID(raster_obj) from raster_table;

——————————————————————————————————
4e692ed0-74e2-42a3-a10d-c28d4ae31982
```

