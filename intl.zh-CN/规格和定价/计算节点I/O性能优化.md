# 计算节点I/O性能优化

云原生数据仓库AnalyticDB PostgreSQL版于2021年5月19日对新建实例的计算节点（Segment节点）I/O性能进行了优化，提升了30%左右的查询速度。

## 发布日期

2021年5月19日。

## 注意事项

-   必须是2021年5月19日以后新建的AnalyticDB PostgreSQL实例。
-   实例资源类型为**存储弹性模式**。

## 计算节点规格与I/O性能提升的关系

|节点规格（Segment）|理论I/O性能提升|
|-------------|---------|
|2C16G|33%|
|4C32G|50%|

## 新旧实例I/O性能对比

用于对比的AnalyticDB PostgreSQL实例Segment节点规格如下：

-   Segment节点：2C16GB
-   Segment节点数量：4
-   Segment节点存储容量：400 GB

新旧实例的总计算资源和存储资源完全相同，且成本保持不变。对总大小80 GB的一个表执行`SELECT COUNT(*)`命令，对比结果如下：

-   2021年5月19日前创建的实例查询结果如下。

    ![旧实例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6165416261/p293858.png)

-   2021年5月19日及以后创建的实例查询结果如下。

    ![新实例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6165416261/p293859.png)


通过以上对比可以看到，在I/O性能提升后，例如COUNT\(\*\)等瓶颈为I/O的查询语句性能，提升了30%左右。

## 存储容量与I/O性能的关系

此处以4C32G规格的实例为例进行测试，对比结果如下。

![存储容量与IO性能的关系](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6165416261/p293868.png)

通过上图可以得出结论，I/O带宽和节点存储容量也存在一定关系。只有存储容量大于阈值，I/O带宽才能达到理论值；阈值以下，I/O带宽和云盘大小存在线性关系。

因此，为达到最大I/O性能，4C32G规格实例的节点存储容量（Segment）建议选择在150GB以上。

2C16G规格的实例也存在上述关系，由于2C16G实例的I/O带宽上限低于4C32G实例，因此Segment节点仅需要选择50GB以上的存储容量即可达到2C16G实例的最大I/O性能。

