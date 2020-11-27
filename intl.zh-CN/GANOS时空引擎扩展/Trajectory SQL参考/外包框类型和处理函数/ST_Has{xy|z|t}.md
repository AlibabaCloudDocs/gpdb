# ST\_Has\{xy\|z\|t\}

确定指定的外包框是否包含某些维度。

## 语法

```
bool ST_HasXY(boxndf box);
bool ST_HasZ(boxndf box);
bool ST_HasT(boxndf box);
```

## 参数

|参数名称|描述|
|----|--|
|box|指定的外包框。|

## 描述

确认指定的外包框是否包含某个或某些维度。由于x维度和y维度为绑定关系，外包框中只会存在全部包含或全不包含的情况。

## 示例

```
postgres=# select ST_hasz(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp));
 st_hasz 
---------
 f

postgres=# select ST_hast(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp));
 st_hast 
---------
 t
```

