# ST\_Srid

获得raster对象的空间参考标识符。空间参考标识符（SRID）的标识以及定义保存在系统表spatial\_ref\_sys。

## 语法

```
integer ST_Srid(raster raster_obj);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|

## 示例

```
select ST_Srid(raster_obj) from raster_table where id=1;

__________________________________
4326
```

