# ST\_lcsSubDistance

计算LCSS轨迹段与traj1在这段轨迹上的距离。

## 语法

```
float8 ST_lcsSubDisatance(trajectory traj1, trajectory traj2, float8 dist, distanceUnit unit default 'M');
float8 ST_lcsSubDisatance(trajectory traj1, trajectory traj2, float8 dist, interval lag, distanceUnit unit default 'M');
```

## 参数

|参数名称|描述|
|----|--|
|traj1|轨迹对象1。|
|traj2|轨迹对象2。|
|dist|两点之间的距离容差，单位为米。|
|lag|两点之间的时间容差。|
|unit|距离单位，允许以下值： -   'M' : 米
-   'KM': 千米
-   'D'： 度，只允许空间参考为 WGS84（4326）轨迹使用 |

## 描述

本函数计算的是与LCSS轨迹段相对应的轨迹1子轨迹的点数与LCSS轨迹段的点数的比例关系。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8629209951/p50874.png)

上图中轨迹点\[2,3,5\]为LCSS轨迹段，轨迹1与此对应的轨迹段为\[2,3,4,5\]，返回的结果为1-3/4。

数值越小表明LCSS轨迹与轨迹1的相似度越高。

通常trajectory对象中会有srid值，如果trajectory对象没有srid值，则默认为4326。

## 示例

```
With traj AS (
    Select ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000528 33.588163 54.87 , 114.000535 33.588235 54.85 , 114.000447 33.588272 54.69 , 114.000348 33.588287 54.73 , 114.000245 33.588305 55.26 , 114.000153 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:30'::timestamp, '2010-01-01 11:31', '2010-01-01 11:32', '2010-01-01 11:33','2010-01-01 11:34','2010-01-01 11:35'], NULL) a,
           ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000529 33.588163 54.87 , 114.000535 33.578235 54.85 , 114.000447 33.578272 54.69 , 114.000348 33.578287 54.73 , 114.000245 33.578305 55.26 , 114.000163 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:29:58'::timestamp, '2010-01-01 11:31:02', '2010-01-01 11:33', '2010-01-01 11:33:09','2010-01-01 11:34','2010-01-01 11:34:30'], NULL) b)
Select st_LCSSubDistance(a, b, 100) from traj;

st_lcssubdistance 
------------------------
 0.66666666666666666667 
 (1 row)

 With traj AS (
    Select ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000528 33.588163 54.87 , 114.000535 33.588235 54.85 , 114.000447 33.588272 54.69 , 114.000348 33.588287 54.73 , 114.000245 33.588305 55.26 , 114.000153 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:30'::timestamp, '2010-01-01 11:31', '2010-01-01 11:32', '2010-01-01 11:33','2010-01-01 11:34','2010-01-01 11:35'], NULL) a,
           ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000529 33.588163 54.87 , 114.000535 33.578235 54.85 , 114.000447 33.578272 54.69 , 114.000348 33.578287 54.73 , 114.000245 33.578305 55.26 , 114.000163 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:29:58'::timestamp, '2010-01-01 11:31:02', '2010-01-01 11:33', '2010-01-01 11:33:09','2010-01-01 11:34','2010-01-01 11:34:30'], NULL) b)
Select st_LCSSubDistance(a, b, 100, interval '30 seconds') from traj;
st_lcssubdistance 
-------------------
0.666666666666667
(1 row)
```

