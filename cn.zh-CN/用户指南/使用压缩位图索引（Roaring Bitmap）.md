# 使用压缩位图索引（Roaring Bitmap） {#concept_628336 .concept}

Bitmap（位图）使用一个bit位来存放数据的某种状态。Bitmap计算（位图计算）利用位级并行可以显著加快查询速度，非常适合大数据基数计算，常用于去重、标签筛选、时间序列等计算中。

Roaringbitmap是一种高效优秀的Bitmap压缩算法，目前已被广泛应用在各种语言和各种大数据平台上。AnalyticDBfor PostgreSQL集成了RoaringBitmap功能，可以将Roaringbitmap作为一种数据类型，提供原生的数据库函数和聚合等功能支持。

## 使用RoaringBitmap位图计算 {#section_8hb_3pg_ijf .section}

使用RoaringBitmap位图计算功能之前，首先需要在数据库中创建RoaringBitmap扩展插件：

``` {#codeblock_33q_yvg_ylo}
create extension roaringbitmap;
```

创建带有RoaringBitmap数据类型的表：

``` {#codeblock_mao_8sl_ezz}
CREATE TABLE t1 (id integer, bitmap roaringbitmap);
```

使用函数插入Bitmap数据：

``` {#codeblock_kw8_hty_glp}
INSERT INTO t1 SELECT 1,RB_BUILD(ARRAY[1,2,3,4,5,6,7,8,9,200]);
INSERT INTO t1 SELECT 2,RB_BUILD_AGG(e) FROM GENERATE_SERIES(1,100) e;
```

Bitmap计算\(OR, AND, XOR, ANDNOT\)

``` {#codeblock_d56_c38_5d0}
SELECT RB_OR(a.bitmap,b.bitmap) FROM (SELECT bitmap FROM t1 WHERE id = 1) AS a,(SELECT bitmap FROM t1 WHERE id = 2) AS b;
```

Bitmap聚合计算\(OR, AND, XOR, BUILD\)

``` {#codeblock_6f9_30g_iea}
SELECT RB_OR_AGG(bitmap) FROM t1;
SELECT RB_AND_AGG(bitmap) FROM t1;
SELECT RB_XOR_AGG(bitmap) FROM t1;
SELECT RB_BUILD_AGG(e) FROM GENERATE_SERIES(1,100) e;
```

统计基数（Cardinality）

``` {#codeblock_v81_dbi_8pu}
SELECT RB_CARDINALITY(bitmap) FROM t1;
```

Bitmap转换为Offset List

``` {#codeblock_v0c_8fr_ixi}
SELECT RB_ITERATE(bitmap) FROM t1 WHERE id = 1;
```

## Bitmap函数列表 {#section_if3_p1i_1v6 .section}

|函数名|输入|输出|描述|示例|
|---|--|--|--|--|
|rb\_build|integer\[\]|roaringbitmap|通过数组创建一个Bitmap。| ``` {#codeblock_04h_yu1_s53}
rb_build('{1,2,3,4,5}')
```

 |
|rb\_and|roaringbitmap，roaringbitmap|roaringbitmap|And计算。| ``` {#codeblock_3zd_hch_sv0}
rb_and(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_or|roaringbitmap，roaringbitmap|roaringbitmap|Or计算。| ``` {#codeblock_27s_227_df9}
rb_or(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_xor|roaringbitmap，roaringbitmap|roaringbitmap|Xor计算。| ``` {#codeblock_sio_12y_0nx}
rb_xor(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_andnot|roaringbitmap，roaringbitmap|roaringbitmap|AndNot计算。| ``` {#codeblock_30u_lsf_imd}
rb_andnot(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_cardinality|roaringbitmap|integer|统计基数| ``` {#codeblock_2u6_1bz_hmg}
rb_cardinality(rb_build('{1,2,3,4,5}'))
```

 |
|rb\_and\_cardinality|roaringbitmap，roaringbitmap|integer|And计算并返回基数。| ``` {#codeblock_vop_ajn_hzj}
rb_and_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_or\_cardinality|roaringbitmap，roaringbitmap|integer|Or计算并返回基数。| ``` {#codeblock_j5l_abe_643}
rb_or_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_xor\_cardinality|roaringbitmap，roaringbitmap|integer|Xor计算并返回基数。| ``` {#codeblock_2mv_q50_cyc}
rb_xor_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_andnot\_cardinality|roaringbitmap，roaringbitmap|integer|AndNot计算并返回基数。| ``` {#codeblock_9n6_r25_yge}
rb_andnot_cardinality(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_is\_empty|roaringbitmap|boolean|判断是否为空的Bitmap。| ``` {#codeblock_2ba_kcb_r6r}
rb_is_empty(rb_build('{1,2,3,4,5}'))
```

 |
|rb\_equals|roaringbitmap，roaringbitmap|boolean|判断两个Bitmap是否相等。| ``` {#codeblock_hos_d6a_shd}
rb_equals(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_intersect|roaringbitmap，roaringbitmap|boolean|判断两个Bitmap是否相交。| ``` {#codeblock_o36_8r0_1e5}
rb_intersect(rb_build('{1,2,3}'),rb_build('{3,4,5}'))
```

 |
|rb\_remove|roaringbitmap,integer|roaringbitmap|从Bitmap移除特定的Offset。| ``` {#codeblock_1mo_vlq_dju}
rb_remove(rb_build('{1,2,3}'),3)
```

 |
|rb\_flip|roaringbitmap,integer,integer|roaringbitmap|翻转Bitmap中特定的Offset段。| ``` {#codeblock_ubn_0qr_pib}
rb_flip(rb_build('{1,2,3}'),2,3)
```

 |
|rb\_minimum|roaringbitmap|integer|返回Bitmap中最小的Offset，如果Bitmap为空则返回-1。| ``` {#codeblock_1wx_9ga_ldr}
rb_minimum(rb_build('{1,2,3}'))
```

 |
|rb\_maximum|roaringbitmap|integer|返回Bitmap中最大的Offset，如果Bitmap为空则返回0。| ``` {#codeblock_aeb_ajd_v5g}
rb_maximum(rb_build('{1,2,3}'))
```

 |
|rb\_rank|roaringbitmap,integer|integer|返回Bitmap中小于等于指定Offset的基数。| ``` {#codeblock_t11_pzc_l4t}
rb_rank(rb_build('{1,2,3}'),3)
```

 |
|rb\_iterate|roaringbitmap|setof integer|返回Offset List。| ``` {#codeblock_zb1_uhl_o7o}
rb_iterate(rb_build('{1,2,3}'))
```

 |

## Bitmap聚合函数列表 {#section_6zl_8ip_shz .section}

|聚合函数名|输入|输出|描述|示例|
|-----|--|--|--|--|
|rb\_build\_agg|integer|roaringbitmap|将Offset聚合成bitmap。| ``` {#codeblock_wo4_ytn_w95}
rb_build_agg(1)
```

 |
|rb\_or\_agg|roaringbitmap|roaringbitmap|Or 聚合计算。| ``` {#codeblock_bt1_jvg_tzv}
rb_or_agg(rb_build('{1,2,3}'))
```

 |
|rb\_and\_agg|roaringbitmap|roaringbitmap|And 聚合计算。| ``` {#codeblock_4b8_bcg_1iw}
rb_and_agg(rb_build('{1,2,3}'))
```

 |
|rb\_xor\_agg|roaringbitmap|roaringbitmap|Xor 聚合计算。| ``` {#codeblock_pko_ikm_lm2}
rb_xor_agg(rb_build('{1,2,3}'))
```

 |
|rb\_or\_cardinality\_agg|roaringbitmap|integer|Or 聚合计算并返回其基数。| ``` {#codeblock_9zb_psm_lvo}
rb_or_cardinality_agg(rb_build('{1,2,3}'))
```

 |
|rb\_and\_cardinality\_agg|roaringbitmap|integer|And 聚合计算并返回其基数。| ``` {#codeblock_lio_ni3_sjg}
rb_and_cardinality_agg(rb_build('{1,2,3}'))
```

 |
|rb\_xor\_cardinality\_agg|roaringbitmap|integer|Xor 聚合计算并返回其基数。| ``` {#codeblock_5cq_z2o_95z}
rb_xor_cardinality_agg(rb_build('{1,2,3}'))
```

 |

