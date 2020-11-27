# ST\_NoData

获取raster对象的某一个波段NoData（无效值标识）的值。如果没有NoData定义，则返回空值。

## 语法

```
 float8 ST_NoData(raster raster_obj, integer band_sn)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|band\_sn|波段序号，从0开始。|

## 示例

```
select ST_NoData(raster_obj, 0) from raster_table where id=1;

__________________________________
0.000
```

