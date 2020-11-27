# ST\_Width

获得raster对象的宽度。如果需要获得分块的宽度，参见ST\_ChunkWidth。

## 语法

```
integer ST_Width(raster raster_obj);
integer ST_Width(raster raster_obj, integer pyramid);
```

## **参数**

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|pyramid|金字塔层级，从0开始。|

## 示例

```
select ST_Width(raster_obj) from raster_table;

——————————————————————————————
10060
```

