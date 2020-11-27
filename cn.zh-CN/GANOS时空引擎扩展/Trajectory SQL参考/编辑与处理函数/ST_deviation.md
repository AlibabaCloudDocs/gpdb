# ST\_deviation

计算处理后的轨迹与原始轨迹之间的偏差。

## 语法

```
float8 ST_deviation(trajectory traj, trajectory after_oper_traj);
```

## 参数

|参数名称|描述|
|----|--|
|traj|原始轨迹对象。|
|after\_oper\_traj|处理后的轨迹对象（比如压缩后）。|

## 示例

```
select st_deviation(traj, st_compress(traj,0.001)) from traj_test;
    st_deviation     
--------------------- 
0.00919177345596219
(1 row)
```

