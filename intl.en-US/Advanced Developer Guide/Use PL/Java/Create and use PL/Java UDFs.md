# Create and use PL/Java UDFs

AnalyticDB for PostgreSQL allows you to compile and upload JAR software packages written in the PL/Java language, and use these JAR packages to create user-defined functions \(UDFs\). AnalyticDB for PostgreSQL supports PL/Java Community Edition 1.5.0 and JVM 1.8.

This topic describes how to create a PL/Java UDF. For more examples of PL/Java code, see [PL/Java code](https://github.com/tada/pljava/tree/master/pljava-examples/src/main/java/org/postgresql/pljava/example). For the compiling method, see [PL/Java documentation](https://tada.github.io/pljava/build/build.html).

## Procedure

1.  In AnalyticDB for PostgreSQL, execute the following statement to create a PL/Java extension. You need to execute the statement only once for each database.

    ```
    create extension pljava;
    ```

2.  Compile the UDF. For example, you can use the following code to compile the `Test.java` file:

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

3.  Compile the `manifest.txt` file.

    ```
     Manifest-Version: 1.0
     Main-Class: Test
     Specification-Title: "Test"
     Specification-Version: "1.0"
     Created-By: 1.7.0_99
     Build-Date: 01/20/2016 21:00 AM
    ```

4.  Run the following commands to compile and package the program:

    ```
     javac Test.java
     jar cfm analytics.jar manifest.txt Test.class
    ```

5.  Upload the `analytics.jar` file that is generated in Step 4 to OSS by using the following OSS console command:

    ```
     osscmd put analytics.jar oss://zzz
    ```

6.  In AnalyticDB for PostgreSQL, execute the CREATE LIBRARY statement to import the file to AnalyticDB for PostgreSQL:

    **Note:** You can only use the filepath variable in the CREATE LIBRARY statement to import files. You can import only one file at a time. You can also execute the CREATE LIBRARY statement to import files as byte streams without the need to use OSS. For more information, see [Use the CREATE LIBRARY statement](/intl.en-US/Advanced Developer Guide/Use PL/Java/Use the CREATE LIBRARY statement.md).

    ```
    create library example language java from 'oss://oss-cn-hangzhou.aliyuncs.com filepath=analytics.jar id=xxx key=yyy bucket=zzz';
    ```

7.  In AnalyticDB for PostgreSQL, execute the following statements to create and use the UDF:

    ```
     create table temp (a varchar) distributed randomly;
     insert into temp values ('my string');
     create or replace function java_substring(varchar, int, int) returns varchar as 'Test.substring' language java;
     select java_substring(a, 1, 5) from temp;
    ```


