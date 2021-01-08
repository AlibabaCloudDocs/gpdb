# ST\_velocityAtTime

This function returns the value of the velocity attribute field at the specified time point.

## Syntax

```
float8 ST_VelocityAtTime (trajectory traj, timestamp t);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|t|The time point.|

## Example

```
Select ST_velocityAtTime(traj, '2010-1-11 23:40:00') From traj_table;
```

