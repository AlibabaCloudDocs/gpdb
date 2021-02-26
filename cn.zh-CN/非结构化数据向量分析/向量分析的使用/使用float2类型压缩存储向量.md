# 使用float2类型压缩存储向量

本节将通过具体示例，为您介绍半浮点数压缩数据列的定义和相关的操作。当前向量检索系统中，会将图片、声音、文本转化成高维浮点数数组进行存储，将占用大量的存储空间。为降低存储成本，压缩存储空间，为您提供了float2压缩存储模式。

## Float2类型简介

半精度浮点数（float2）是一种被计算机使用的二进制浮点数据类型。半精度浮点数使用2个字节（16位）来存储，来存储之前4个字节（32位）的float4的数据。IEEE 754标准指定了一个binary16需具备如下的格式：

-   Sign bit（符号位）： 1 bit。
-   Exponent width（指数位宽）： 5 bits。
-   Significand precision（尾数精度）： 11 bits （有10位被显式存储）。

按如下顺序排列：

![float2数据类型](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5929204951/p86390.png)

除非指数位全是0，否则就会假定隐藏的起始位是1。因此只有10位尾数在内存中被显示出来，而总精度是11位。根据IEEE 754标准，虽然尾数只有10位，但是尾数精度是11位的（log10\(211\)≈ 3.311 十进制数）。

```
0 01111 0000000000 = 1
0 01111 0000000001 = 1 + 2−10 = 1.0009765625 （1之后的最接近的数）
1 10000 0000000000 = −2 

0 11110 1111111111 = 65504  （max half precision） 

0 00001 0000000000 = 2−14 ≈ 6.10352 × 10−5 （最小正指数）
0 00000 1111111111 = 2−14 - 2−24 ≈ 6.09756 × 10−5 （最大尾数）
0 00000 0000000001 = 2−24 ≈ 5.96046 × 10−8 （最小正尾数） 

0 00000 0000000000 = 0
1 00000 0000000000 = −0 

0 11111 0000000000 = infinity
1 11111 0000000000 = −infinity 

0 01101 0101010101 = 0.333251953125 ≈ 1/3
```

由于尾数的位数是奇数，所以默认情况下，类似1/3的数会像双精度浮点数一样四舍五入。

对于float2和float4之间的转换，除了不同部分的移位之外，还需要注意指数的基数之间的差别（15和127）。例如，要把float2类型转换为float4类型，主要进行以下几步操作。

1.  符号位左移16位。
2.  指数部分加112（127与15之间的差距），左移13位（右对齐）。
3.  尾数部分左移13位（左对齐）。

**说明：** Float4转换为float2的步骤与之相反。

因此当前的浮点数的压缩是损失精度的压缩，所以在进行查询计算的时候会有一定的精度的损失。在实际应用中，这种损失是满足业务的要求的。

Float2压缩存储是用两个字节，来表示之前的四个字节的存储，所以对于向量列的压缩比例在0.5，即占用磁盘空间是原来的50%。

Float2类型只能表达\[-65519.99, 65519.99\]之间的值。如果超过取值范围，比方说大于65519，系统会输出Infinity，如果小于-65519，系统会输出-Infinity。对于向量检索来说，向量需要进行归一化处理，将取值范围归一化到\[0,1\]之间。不进行归一化的向量距离计算，会非常容易超过取值范围，导致距离计算的不准确。

对于向量float2与float4类型之间的相互转化，会有一定的性能上的消耗。当前float2的数组类型转换，实现了两种转换算法：

-   针对数组中的每个float2的数据，使用C程序进行转化，每次只转换一个float2数据。
-   对于特定的硬件（支持AVX和SSE2指令集的硬件），调用硬件特定的接口函数，每次可以支持同时转换4个float2类型。

在实际的查询的过程中，因为会用到索引等相关的遍历技术，所以不用转换很多记录。

## 创建使用float2数据类型的表

Float2是内部定义的一个数据类型，系统实现了各种类型的转换，以及相关的各种操作符。因此，在实际系统中，一般将float2数据类型当成基本数据类型来进行相关的操作。

语法：

```
CREATE TABLE [TABLE_NAME]
(  
    C1 INT,  
    C2 FLOAT2[],  
    C3 VARCHAR(20),  
    PRIMARY KEY(C1)
);
```

**说明：** C2即float2向量存储列。

示例：

在FACE\_TABLE表中，创建float2的向量列C2。

```
CREATE TABLE FACE_TABLE (  
    C1 INT PRIMARY KEY,  
    C2 FLOAT2[],  
    C3 VARCHAR(20)
);
```

## 插入数据

对已经建立好的float2类型的数组，插入相关的数据。可以用下述三种方式对float2的数组插入数据。在进行数据插入的时候，用户可以显示的定义出float2的数组，将相关的数据插入到表中（参见下述代码中的sql1）；或者用户采用隐示的类型转换，系统会在内部将float4类型的数组，转换成float2类型的数组，存储到对应的表中（参见下述代码中的sql2和sql3）。

示例：

插入三条数据到创建的FACE\_TABLE中。

```
sql1 = INSERT INTO FACE_TABLE (C1, C2, C3)
    VALUES (1, ARRAY[1.3, 2.4, 5.6]::FLOAT2[], 'name1'); 

sql2 = INSERT INTO FACE_TABLE (c1, c2, c3) 
    VALUES (2, ARRAY [3.4, 6.1, 7.6]::REAL[], 'name2'); 

sql3 = INSERT INTO FACE_TABLE (c1, c2, c3) 
    VALUES (3, ARRAY [9.5, 1.2, 0.6]::FLOAT4[],'name3');
```

## 查询数据

由于采用的是float2类型的数据，所以在显示查询结果时有一定的数据精度丢失。例如插入的是1.3，而实际查询的结果是1.2998；或者插入的是5.6，而实际查询的结果是5.60156。这种精度的损失对于向量检索来说，是可以忽略不计的。

示例：

```
SELECT * FROM FACE_TABLE; 
c1  |            c2             |  c3   
----+---------------------------+-------
  1 | {1.2998,2.40039,5.60156}  | name1
  2 | {3.40039,6.10156,7.60156} | name2
  3 | {9.5,1.2002,0.600098}     | name3
```

## float2表数据的压缩比例

本示例中，建立两张表，一个是用float4类型的向量数据，一个是float2类型的向量数据，对比实际表的大小。

```
--CREATE TABLE 
CREATE TABLE TAB1(A FLOAT4[]);
CREATE TABLE TAB2(A FLOAT2[]); 

--INSERT DATA 
INSERT INTO TAB1 
SELECT GEN_RAND_F2_ARR (1, 1024) FROM GENERATE_SERIES (1,10000);
INSERT INTO TAB2 
SELECT GEN_RAND_F2_ARR (1, 1024) FROM GENERATE_SERIES (1,10000); 

--QUERY SIZE
SELECT PG_SIZE_PRETTY (PG_RELATION_SIZE('tab1'));
 PG_SIZE_PRETTY 
----------------
 45 MB(1 row)
 
SELECT PG_SIZE_PRETTY (PG_RELATION_SIZE('tab2')); 
 PG_SIZE_PRETTY
----------------
 21 MB(1 row)
```

从上述信息可查看到，使用float4数据类型的存储是45M，使用float2类型的数据存储是21M。由此可见，float2的存储大约是float4的一半。

## float2表数据的压缩和解压的性能比较

当前系统提供了两个函数来进行float2与float4相互的转换：array\_f16\_to\_f32将float2类型的向量转化成float4类型的向量，array\_f32\_to\_f16将float4类型的向量转化成float2的向量。当前每个向量的长度是1024维，是在支持AVX和SSE2的指令集的机器上面进行测试的。

示例：

```
--CREATE TABLE 
CREATE TABLE TAB1(A FLOAT4[]);
CREATE TABLE TAB2(A FLOAT2[]); 

--INSERT TABLE
INSERT INTO TAB1 SELECT GEN_RAND_F2_ARR(1, 1024) FROM GENERATE_SERIES (1,10000);
INSERT INTO TAB2 SELECT GEN_RAND_F2_ARR(1, 1024) FROM GENERATE_SERIES (1,10000); 

\TIMING
--query size
SELECT ARRAY_F32_TO_F16(a) FROM TAB1;  
    Time: 5998.832 ms (00:05.999)
SELECT ARRAY_F16_TO_F32(a) FROM TAB2;
    Time: 5507.388 ms (00:05.507) 
```

## 距离计算

为了方便距离计算，当前的系统针对float2\[\]类型，提供了L2距离计算，系统在内部会将float2类型的数据，隐式的转成float4类型的数据，来计算相关的距离。

示例：

计算l2距离。

```
SELECT L2_DISTANCE(ARRAY[1.0,1.0,1.0,1.0,1.0,1.0,1.0,1.0,1.0]::FLOAT2[], 
    ARRAY[0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0]::FLOAT2[]); 

SELECT L2_DISTANCE(ARRAY[1.0,1.0,1.0,1.0,1.0,1.0,1.0,1.0,1.0]:: FLOAT4[], 
    ARRAY [0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0]:: FLOAT2[]); 

SELECT L2_DISTANCE (ARRAY[1.0,1.0,1.0,1.0,1.0,1.0,1.0,1.0,1.0]:: FLOAT2[], 
    ARRAY [0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0]::FLOAT2[]);
```

## float2的实际应用案例

对于安保系统来说，每天都会定时的将监控的图片数据存在人脸表中，安保系统会输入人脸的照片，在监控系统中查找相关的监控图片。下文将介绍float2在查询检索的应用。

1.  创建一个表，用于存放人脸识别的相关数据。

    ```
    CREATE TABLE FACE_TABLE (
      C1 INT PRIMARY KEY,
      C2 FLOAT2[],
      C3 VARCHAR(20)
    );
    ```

    **说明：**

    -   C1：人脸的编号。
    -   C2：人脸的向量。
    -   C3：对应的人名。
2.  在FACE\_TABLE表中建立向量索引。

    ```
    CREATE INDEX FACE_TABLE_IDX 
    ON FACE_TABLE 
    USING ANN(C2) WITH(dim=10);
    ```

3.  导入相关的监控数据到FACE\_TABLE表中。

    ```
    INSERT INTO FACE_TABLE (C1, C2, C3)  
    VALUES (1, ARRAY[1.3, 2.4, 5.6]::FLOAT2[], 'name1'); 
    
    INSERT INTO FACE_TABLE (c1, c2, c3) 
    VALUES (2, ARRAY[3.4, 6.1, 7.6]::REAL[], 'name2'); 
    
    INSERT INTO FACE_TABLE (c1, c2, c3) 
    VALUES (3, ARRAY[9.5, 1.2, 0.6]::FLOAT4[],'name3');
    ```

4.  输入人脸的数据，进行向量查询。

    ```
    SELECT * 
    FROM FACE_TABLE 
    ORDER BY C1 <-> ARRAY[2.81574,9.84361,8.07218]:: FLOAT2[] 
    LIMIT 10;
    ```

    **说明：** `ARRAY[2.81574,9.84361,8.07218]:: FLOAT2[]`表示需要查询的图片向量，系统会在底库中检索对应的人脸信息。


