# ST\_accelerationAtTime

This function returns the value of the acceleration attribute field at the specified time point.

## Syntax

```
float8 ST_accelerationAtTime (trajectory traj, timestamp t);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|t|The specified time point.|

## Example

```
Select ST_accelerationAtTime(traj,'2010-1-11 23:40:00') From traj_table;
```

