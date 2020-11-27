# ST\_ChunkWidth

获得raster对象分块的宽度。

## 语法

```
integer ST_ChunkWidth(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 描述

获得raster对象分块的宽度，单位：像素。

## 示例

```
select ST_ChunkWidth(rast)
from raster_table;

 st_chunkwidth  
----------
      256 
```

