# ST\_Name

获得raster对象的名称。如果没有定义名称，则返回空值。

## 语法

```
text ST_Name(raster rast);
```

## 参数

|参数名称|描述|
|----|--|
|rast|raster对象。|

## 示例

```
select ST_Name(rast) from rat where id=1;

__________________________________
image1
```

