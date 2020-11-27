# ST\_CheckGPU

验证是否有GPU环境。

## 语法

```
text  ST_CheckGPU();
```

## 描述

验证当前数据库运行环境是否有可识别的GPU硬件设备。

## 示例

```
select st_checkgpu();
                                      st_checkgpu                                       
----------------------------------------------------------------------------------------
 [GPU prop]multiProcessorCount=20; sharedMemPerBlock=49152; maxThreadsPerBlock=1024
 
(1 row)
```

