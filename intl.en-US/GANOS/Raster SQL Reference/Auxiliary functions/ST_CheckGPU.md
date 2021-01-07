# ST\_CheckGPU

This function checks whether a GPU is available in the current environment.

## Syntax

```
text ST_CheckGPU();
```

## Description

This function checks whether a GPU can be identified in the current runtime environment of ApsaraDB for RDS.

## Examples

```
select st_checkgpu();
                                      st_checkgpu                                       
----------------------------------------------------------------------------------------
 [GPU prop]multiProcessorCount=20; sharedMemPerBlock=49152; maxThreadsPerBlock=1024
 
(1 row)
```

