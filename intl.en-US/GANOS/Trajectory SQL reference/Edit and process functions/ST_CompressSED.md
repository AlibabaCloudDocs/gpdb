# ST\_CompressSED

This function compresses a trajectory object based on the offset threshold of the synchronous Euclidean distance \(SED\).

## Syntax

```
trajectory ST_CompressSed (trajectory traj, float8 dist);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The original trajectory object.|
|dist|The SED offset threshold. After this value is specified, trajectory points whose SED offset is greater than this value are kept, so as to maintain the spatial trend of the original trajectory object.|

## Description

This function calculates the SED of trajectory points, discards the points whose SED offset is smaller than the SED offset threshold to compress the trajectory object in lossy mode, and then returns the compressed trajectory object.

## Examples

```
select st_compressSED(traj, 0.001) as traj from traj_test;
           traj
-------------------------------------------------------------
{"trajectory":{"version":1,"type":"STPOINT","leafcount":12,"start_time":"2017-01-15 09:06:39","end_time":"2017-01-15 21:18:30","spatial":"LINESTRING(-179.48077 51.72814,-179.42595 51.80094,-179.39734 51.83398,-179.37474 51.86568,-179.18826 52.10105,-179.16482 52.12996,-179.14599 52.15132,-177.76666 52.85042,-177.47319 52.90084,-177.31238 52.92697,-177.03751 52.97394,-176.68481 53.03327)","timeline":["2017-01-15 09:06:39","2017-01-15 09:34:40","2017-01-15 09:47:38","2017-01-15 09:59:09","2017-01-15 11:30:09","2017-01-15 11:42:00","2017-01-15 11:50:28","2017-01-15 18:01:00","2017-01-15 18:56:22","2017-01-15 19:26:10","2017-01-15 20:15:49","2017-01-15 21:18:30"],"attributes":{"leafcount":12,"sog":{"type":"float","length":8,"nullable":false,"value":[10.5,11.0,10.6,11.2,9.1,9.7,9.9,12.3,12.0,12.2,12.8,12.7]},"cog":{"type":"float","length":8,"nullable":false,"value":[23.3,28.5,29.2,27.1,26.8,30.1,30.0,74.2,81.3,73.4,74.2,72.9]},"heading":{"type":"float","length":8,"nullable":false,"value":[22.0,27.0,24.0,29.0,27.0,26.0,27.0,69.0,72.0,71.0,72.0,73.0]}}}}
(1 row)
```

