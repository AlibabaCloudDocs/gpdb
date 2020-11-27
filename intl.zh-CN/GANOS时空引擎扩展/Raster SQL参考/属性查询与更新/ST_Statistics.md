# ST\_Statistics

获取raster对象的某一个波段的统计值信息的JSON格式。如果不存在统计值，则返回空值。

## 语法

```
text ST_Statistics(raster raster_obj, integer band);
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|Raster对象。|
|band|波段序号，从0开始。|

## 示例

```
select ST_Statistics(raster_obj, 0) from raster_table where id=1;

__________________________________
'{    "min" : 0.00, "max" : 255.00,"mean" : 125.00,"std" : 23.123,"approx" : false}'
```

