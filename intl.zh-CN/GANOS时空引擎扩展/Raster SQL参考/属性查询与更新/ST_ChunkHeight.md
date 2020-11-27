# ST\_ChunkHeight

获得raster对象分块的高度。

## 语法

```
integer ST_ChunkHeight(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 描述

获得raster对象分块的高度，单位：像素。

## 示例

```
select ST_ChunkHeight(rast)
from raster_table;

 st_chunkheight  
----------
      256 
```

