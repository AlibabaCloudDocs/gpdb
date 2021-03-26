# 使用Zhparser支持中文分词

本文介绍了如何在AnalyticDB PostgreSQL版中使用Zhparser支持中文分词。全文检索功能仅支持AnalyticDB PostgreSQL 6.0版。

## 概述

PostreSQL默认分词是按照空格及各种标点符号来分词，不支持中文分词，AnalyticDB PostgreSQL版通过集成Zhparser扩展来支持中文分词。

PostreSQL官方文档链接：

-   全文检索总体介绍：[http://www.postgres.cn/docs/9.4/textsearch.html](http://www.postgres.cn/docs/9.4/textsearch.html)
-   全文检索函数和操作符：[http://postgres.cn/docs/9.4/functions-textsearch.html](http://postgres.cn/docs/9.4/functions-textsearch.html)

通常，可采用以下方法进行全文搜索：

1.  搜索表：

    ```
    SELECT name FROM <table...>
    WHERE to_tsvector('english', name) @@ to_tsquery('english', 'friend');
    ```

2.  创建GIN索引：

    ```
    CREATE INDEX <idx_...> ON <table...> USING gin(to_tsvector('english', name));
    ```


## 使用Zhparser

1.  安装Zhparser扩展。在superuser下执行以下命令。

    ```
    create extension zhparser;
    ```

2.  配置中文解析器，可以通过`\dF`和`\dFp`命令查看配置，目前暂不支持用户自定义词典。

    ```
    create text search configuration zh_cn (PARSER = zhparser);
    
    --# 普遍情况下只需要按照名词(n)，动词(v)，形容词(a)，成语(i),叹词(e)和习用语(l) 6种分词策略配置分词
    --# 使用内置的simple词典配置
    alter text search configuration zh_cn add mapping for n,v,a,i,e,l with simple;
    ```

3.  查看分词策略。

    ```
    --# ts_token_types
    select ts_token_type('zhparser');
    
    ---------------------------------
              ts_token_type          
    ---------------------------------
     (97,a,"adjective,形容词")
     (98,b,"differentiation,区别词")
     (99,c,"conjunction,连词")
     (100,d,"adverb,副词")
     (101,e,"exclamation,感叹词")
     (102,f,"position,方位词")
     (103,g,"root,词根")
     (104,h,"head,前连接成分")
     (105,i,"idiom,成语")
     (106,j,"abbreviation,简称")
     (107,k,"tail,后连接成分")
     (108,l,"tmp,习用语")
     (109,m,"numeral,数词")
     (110,n,"noun,名词")
     (111,o,"onomatopoeia,拟声词")
     (112,p,"prepositional,介词")
     (113,q,"quantity,量词")
     (114,r,"pronoun,代词")
     (115,s,"space,处所词")
     (116,t,"time,时语素")
     (117,u,"auxiliary,助词")
     (118,v,"verb,动词")
     (119,w,"punctuation,标点符号")
     (120,x,"unknown,未知词")
     (121,y,"modal,语气词")
     (122,z,"status,状态词")
     
     --# 查看zh_cn configuration
     select * from pg_ts_config_map 
     where mapcfg=(select oid from pg_ts_config where cfgname='zh_cn');
    ```

4.  添加或删除分词策略。

    ```
    --# 新增
    alter text search configuration zh_cn add mapping for m,q,t with simple;
    
    --# 删除
    alter text search configuration zh_cn drop mapping if exists for m,q,t;
    ```

5.  完成安装扩展和配置解析器，即可使用中文分词功能。

    ```
    select to_tsvector('zh_cn', '有两种方法进行全文检索');
    select to_tsquery('zh_cn', '有两种方法进行全文检索');
    
    --# 如果要支持短词复合: 1
    set zhparser.multi_short = on; 
    ```


## 示例

```
--# 创建表
create table zh_test (id int, name text) distributed by (id);

--# 插入数据
insert into zh_test values (1, '北京大学'), (2, '清华大学'), (3, '人民大学'), (4, '地质大学');

--# 创建索引
set zhparser.multi_short = on;
create index idx_test on zh_test using gin(to_tsvector('zh_cn', coalesce(name,'')));

--# 全文检索
select * from zh_test 
where to_tsvector('zh_cn', coalesce(name,'')) @@ to_tsquery('zh_cn', '大学')
order by ts_rank(to_tsvector('zh_cn', coalesce(name,'')), to_tsquery('zh_cn', '大学'));


--# 简化方法，增加一列tsvector类型
alter table zh_test add column tsv tsvector;
set zhparser.multi_short = on;
update zh_test set tsv = to_tsvector('zh_cn', coalesce(name,''));
create index idx_test1 on zh_test using gin(tsv);
select * from zh_test where tsv @@ to_tsquery('zh_cn', '大学');
```

