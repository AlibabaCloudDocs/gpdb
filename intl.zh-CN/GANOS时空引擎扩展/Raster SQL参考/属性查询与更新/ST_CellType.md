# ST\_CellType

获得raster对象的像素类型。类型值可以为以下值之一："8BSI"， "8BUI"， "16BSI"， "16BUI"， "32BSI"， "32BUI"， "32BF"， "64BF"。

## 语法

```
text ST_CellType(raster raster_obj);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|

## 示例

```
select st_celltype(raster_obj) from raster_table;

__________________________________
8BUI
```

