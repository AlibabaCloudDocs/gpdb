# ST\_InterleavingType

获得raster对象的交错类型。交错类型可以是以下其中的一种："BSQ"，"BIL"，"BIP"。

## 语法

```
text ST_InterleavingType(raster raster_obj);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|

## 示例

```
select ST_InterleavingType(raster_obj) from raster_table;

__________________________________
BSQ
```

