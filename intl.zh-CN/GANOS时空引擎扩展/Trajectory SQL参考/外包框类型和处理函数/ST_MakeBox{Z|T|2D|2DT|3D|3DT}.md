# ST\_MakeBox\{Z\|T\|2D\|2DT\|3D\|3DT\}

直接写入值来构建Box。

## 语法

```
boxndf ST_MakeBoxZ(float8 zmin, float8 zmax);
boxndf ST_MakeBoxT(timestamp tmin, timestamp tmax);
boxndf ST_MakeBox2D(float8 xmin, float8 ymin, float8 xmax, float8 ymax);
boxndf ST_MakeBox2DT(float8 xmin, float8 ymin, timestamp tmin, float8 xmax,float8 yax, timestamp tmax);
boxndf ST_MakeBox3D(float8 xmin, float8 ymin, float8 zmin, float8 xmax, float8 ymax, float8 zmax);
boxndf ST_MakeBox3DT(float8 xmin, float8 ymin, float8 zmin, timestamp tmin, float8 xmax, float8 ymax, float8 zmax, timestamp tmax);
```

## 参数

|参数名称|描述|
|----|--|
|xmin|外包框的x轴下界。|
|xmax|外包框的x轴上界。|
|ymin|外包框的y轴下界。|
|ymax|外包框的y轴上界。|
|zmin|外包框的z轴下界。|
|zmax|外包框的z轴上界。|
|tmin|外包框的时间轴下界。|
|tmax|外包框的时间轴上界。|

## 描述

根据函数名和指定的参数构建外包框。

由于外包框内部由float类型表示，因此得到的外包框可能会略大于输入的参数，例如下界比实际值略小或上界比实际值略大。

## 示例

```
SELECT ST_MakeBox2d(0,0,3,3);
  st_makebox2d  
----------------
 BOX2D(0 0,3 3)
 
 SELECT ST_MakeBox3dt(0,0,3,'2000-01-01 00:00:03'::timestamp, 2,5,4,'2000-01-01 02:46:40'::timestamp);
                               st_makebox3dt                               
---------------------------------------------------------------------------
 BOX3DT(0 0 3 2000-01-01 00:00:02.999999,2 5 4 2000-01-01 02:46:40.000476)
(1 row)
```

