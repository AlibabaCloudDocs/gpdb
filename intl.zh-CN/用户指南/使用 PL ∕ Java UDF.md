# 使用 PL ∕ Java UDF {#concept_rpl_x3z_52b .concept}

AnalyticDB for PostgreSQL 支持用户使用 PL/Java 语言，编写并上传 jar 软件包，并利用这些 jar 包创建用户自定义函数（UDF）。该功能支持的 PL/Java 语言版本为社区版 PL/Java 1.5.0，使用的 JVM 版本为 1.8。

本文介绍了创建 PL/Java UDF 的示例步骤。更多的 PL/Java 样例，请参见 [PL/Java代码](https://github.com/tada/pljava/tree/master/pljava-examples/src/main/java/org/postgresql/pljava/example)（查看 [编译方法](https://tada.github.io/pljava/build/build.html)）。

## 操作步骤 { .section}

1.  在 AnalyticDB for PostgreSQL 中，执行如下命令，创建 PL/Java 插件（每个数据库只需执行一次）。

    ```
    create extension pljava;
    ```

2.  根据业务需要，编写自定义函数。例如，使用如下代码编写 `Test.java` 文件：

    ```
     public class Test
     {
         public static String substring(String text, int beginIndex,
                   int endIndex)
                 {
                         return text.substring(beginIndex, endIndex);
                 }
     }
    ```

3.  编写 `manifest.txt` 文件。

    ```
     Manifest-Version: 1.0
     Main-Class: Test
     Specification-Title: "Test"
     Specification-Version: "1.0"
     Created-By: 1.7.0_99
     Build-Date: 01/20/2016 21:00 AM
    ```

4.  执行如下命令，将程序编译打包。

    ```
     javac Test.java
     jar cfm analytics.jar manifest.txt Test.class
    ```

5.  将步骤 4 生成的 `analytics.jar` 文件，通过 OSS 控制台命令上传到 OSS。

    ```
     osscmd put analytics.jar oss://zzz
    ```

6.  在 AnalyticDB for PostgreSQL 中，执行 Create Library 命令，将文件导入到 AnalyticDB for PostgreSQL 中。

    **说明：** Create Library 只支持 filepath，一次导入一个文件。另外，Create Library 还支持字节流形式，可以不通过 OSS 直接导入，详情请参见[Create Library命令的使用](intl.zh-CN/用户指南/使用 Create Library 命令.md#) 。

    ```
    create library example language java from 'oss://oss-cn-hangzhou.aliyuncs.com filepath=analytics.jar id=xxx key=yyy bucket=zzz';
    ```

7.  在 AnalyticDB for PostgreSQL 中，执行如下命令，创建和使用相应 UDF。

    ```
     create table temp (a varchar) distributed randomly;
     insert into temp values ('my string');
     create or replace function java_substring(varchar, int, int) returns varchar as 'Test.substring' language java;
     select java_substring(a, 1, 5) from temp;
    ```


