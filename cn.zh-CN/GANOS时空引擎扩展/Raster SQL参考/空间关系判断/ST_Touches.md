# ST\_Touches

判断raster和raster或raster和geometry的空间关系。

## 语法

```
bool  ST_Touches(raster rast1, raster rast2);
bool  ST_Touches(raster rast, geometry geom);
bool  ST_Touches(geometry geom, raster rast);
```

## 参数

|参数名称|描述|
|----|--|
|rast1|raster对象1。|
|rast2|raster对象2。|
|rast|raster对象。|
|geom|geometry对象。|

## 示例

```
SELECT a.id
FROM tbl_a a, tbl_b b
WHERE ST_Touches(a.rast, b.rast)
```

