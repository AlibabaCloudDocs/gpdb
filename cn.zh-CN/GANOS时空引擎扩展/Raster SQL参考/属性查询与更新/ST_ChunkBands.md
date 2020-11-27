# ST\_ChunkBands

获得raster对象分块波段维数量。

## 语法

```
integer ST_ChunkBands(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 示例

```
select ST_ChunkBands(rast)
from raster_table;

 st_chunkbands  
----------
      3
```

