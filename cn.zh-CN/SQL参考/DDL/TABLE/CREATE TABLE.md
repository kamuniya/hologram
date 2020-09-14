---
keyword: [创建表, Hologres, CREATE TABLE]
---

# CREATE TABLE

CREATE TABLE语句用于创建表。本文为您介绍在交互式分析Hologres中CREATE TABLE的用法。

## 建表

1.  **命令格式**

    目前Hologres 语法是PostgreSQL的一个子集，支持的建表语法如下。

    ```
    begin;
    create table [if not exists] [schema_name.]table_name ([
      {
       column_name column_type [column_constraints, [...]]
       | table_constraints
       [, ...]
      }
    ]);
    call set_table_property('<table_name>', property, value);
    commit;
    ```

2.  **参数说明**
    1.  column\_type：为字段的数据类型，已支持的数据类型可以参见[数据类型](/cn.zh-CN/SQL参考/数据类型.md)。
    2.  列约束`column_constraints`和表约束`table_constraints`的支持情况如下。

        |column\_constraints|table\_constraints|
        |-------------------|------------------|
        |primary key|支持|
        |not null|支持|
        |null|支持|
        |unique|不支持|
        |check|不支持|
        |default|不支持|

        **说明：** `column_constraints`不能有多列为`primary key`，但`table_constraints`允许设置多列为表`primary key`。

    3.  set\_table\_property为表设置属性，详见下文。
3.  **使用限制**
    1.  表名和列名均对大小写不敏感，如需定义大写表名、大写列名、特殊字符表名或列名，可使用双引号`““`进行转义。示例如下：

        ```
        create table "TBL"(a int);
        select relname from pg_class where relname = 'TBL';
        insert into "TBL" values (-1977);
        select * from "TBL";
        ------------------------------------------------------------------
        begin;
        create table tbl ("C1" int not null);
        call set_table_property('tbl', 'clustering_key', '"C1"');
        commit;
        ------------------------------------------------------------------
        begin;
        create table tbl ("C1" int not null, c2 text);
        call set_table_property('tbl', 'clustering_key', '"C1,c2:desc"');  -- set_table_property 见下文
        call set_table_property('tbl', 'segment_key', '"C1",c2:desc');
        commit;
        ------------------------------------------------------------------
        create table "Tab_$A%*" (a int);
        select relname from pg_class where relname = 'Tab_$A%*';
        insert into "Tab_$A%*" values (-1977);
        select * from "Tab_$A%*";
        ```

    2.  `IF NOT EXISTS`：在创建表时，如果不存在同名表且语义正确，表创建都会返回成功。如果不指定`IF NOT EXISTS`选项而存在同名表，则返回异常。如果指定`IF NOT EXISTS`选项，Hologres会提示信息，跳过表创建步骤，返回成功，直观的规则如下。

        |指定if not exists|不指定if not exists|
        |---------------|----------------|
        |存在同名表|`NOTICE：relation “xx“already exists，skippingSUCCEED`|
        |不存在同名表|`SUCCEED`|

    3.  如未做双引号““转义，则表名、列名中不能有特殊字符，只能用英文的a-z、A-Z及数字和下划线（），且必须以字母开头。由于大小写不敏感，A-Z统一会被看成小写，示例如下。

        ```
        begin;
        create table TABLE_one (a int not null);
        call set_table_property('table_one', 'clustering_key', 'a');
        commit;
        insert into table_one values (12);
        ```

    4.  表名的长度不超过64字节，超过64字节将被截断。
4.  **使用示例**

    示例建一张含主键的普通表。

    ```
    CREATE TABLE test (
     "id" bigint NOT NULL,
     "name" text NOT NULL,
     "age" bigint,
     "class" text NOT NULL,
    PRIMARY KEY (id)
    );
    ```


## 设置表属性

在Hologres中，可以通过set\_table\_property为表设置多种属性，合理的表属性设置可以有助于系统高效地组织和查询数据.

1.  **命令格式**

    ```
    call set_table_property('<table_name>', property, value)
    ```

    **说明：** ：`set_table_property`的调用需要与`create table`在同一事务中执行。

    Hologres当前版本支持的设置表属性有以下几种。

    ```
    call set_table_property('table_name', 'orientation', '[column | row]'); 
    call set_table_property('table_name', 'clustering_key', '[columnName{:[desc|asc]} [,...]]'); 
    call set_table_property('table_name', 'segment_key', '[columnName [,...]]');
    call set_table_property('table_name', 'bitmap_columns', '[columnName [,...]]');
    call set_table_property('table_name', 'dictionary_encoding_columns', '[columnName [,...]]');
    call set_table_property('table_name', 'time_to_live_in_seconds', '<non_negative_literal>');
    call set_table_property('table_name', 'distribution_key', '[columnName[,...]]');
    ```

2.  **参数说明**
    1.  **orientation**

        ```
        call set_table_property('<table_name>', 'orientation', '[column | row]');
        ```

        -   orientation：指定了数据库表在Hologres中的存储模式是列存还是行存。
        -   在Hologres中表默认为列存（column store）形式。**列存对于OLAP场景较为友好，适合各种复杂查询，行存对于kv场景比较友好，适合基于primary key的点查和扫描scan**。
        -   使用示例

            ```
            begin;
            create table tbl (a int not null, b text not null);
            call set_table_property('tbl', 'orientation', 'row');
            commit;
            ```

    2.  **clustering key**

        ```
        call set_table_property('<table_name>', 'clustering_key', '[columnName{:[desc|asc]} [,...]]');
        ```

        -   clustering\_key：在指定的列上建立聚簇索引。Hologres会在聚簇索引上对数据进行排序，建立聚簇索引能够加速在索引列上的range和filter查询。
        -   clustering\_key指定的列必须满足非空约束（not null）。
        -   clustering\_key指定列时，可在列名后添加 ：`desc`或者`asc`来表明构建索引时的排序方式。排序方式默认为`asc`，即升序。
        -   数据类型为`float/double`的列，不能设置为`clustering_key`。
        -   使用示例

            ```
            begin;
            create table tbl (a int not null, b text not null);
            call set_table_property('tbl', 'clustering_key', 'a,b');
            commit;
            -------------------------------------------------------------
            begin;
            create table tbl (a int not null, b text not null);
            call set_table_property('tbl', 'clustering_key', 'a:desc,b:asc');
            commit;
            ```

    3.  **segment\_key**

        ```
        call set_table_property('<table_name>', 'segment_key', '[columnName{:[desc|asc]} [,...]]');
        ```

        -   segment\_key：指定一些列（例如，时间列）作为分段键，当查询条件包含分段列时，查询可以通过segment\_key快速找到相应数据对应的存储位置。
        -   设置segment\_key要求orientation 为column，即列存表。
        -   segment\_key指定的列必须满足非空约束（not null）。
        -   float / double等浮点类型列不能被设置为 segment\_key。
        使用示例

        ```
        begin;
        create table tbl (a int not null, b text not null);
        call set_table_property('tbl', 'segment_key', 'a,b');
        commit;
        -------------------------------------------------------------
        begin;
        create table tbl (a int not null, b time not null);
        call set_table_property('tbl', 'segment_key', 'b:asc');
        commit;
        ```

    4.  **bitmap columns**

        ```
        call set_table_property('<table_name>', 'bitmap_columns', '[columnName [,...]]');
        ```

        -   bitmap\_columns：比特编码列。bitmap可以对segment内部的数据进行快速过滤，所以建议把filter条件的数据建成比特编码。
        -   设置bitmap\_columns要求表的存储形式为column，即列存表。
        -   bitmap\_columns适合无序且取值不多的列，对于每个取值构造一个二进制串，表示取值所在位置的bitmap。
        -   bitmap\_columns指定的列可以为null。
        -   默认所有text列都会被隐式地设置到bitmap\_columns中。
        -   **可以再事务之外单独使用，表示修改bitmap\_columns列。**
        -   使用示例

            ```
            begin;
            create table tbl (a int not null, b text not null);
            call set_table_property('tbl', 'bitmap_columns', 'a,b');
            commit;
            ```

    5.  **dictionary encoding columns**

        ```
        call set_table_property('<table_name>', 'dictionary_encoding_columns', '[columnName [,...]]');
        ```

        -   dictionary\_encoding\_columns：字典编码列，为指定列的值构建字典映射。字典编码可以将字符串的比较转成数字的比较，加速group by、filter等查询。
        -   设置dictionary\_encoding\_columns要求表的存储形式为column，即列存表。
        -   dictionary\_encoding\_columns指定的列可以为null。
        -   无序但取值较少的列适合设置dictionary\_encoding\_columns，可以压缩存储。
        -   默认所有text列都会被隐式地设置到dictionary\_encoding\_columns中。
        -   可以再事务之外单独使用。表示修改dictionary\_encoding\_columns列。
        -   使用示例

            ```
            begin;
            create table tbl (a int not null, b text not null);
            call set_table_property('tbl', 'dictionary_encoding_columns', 'a,b');
            commit;
            ```

    6.  **time to live in seconds**

        ```
        call set_table_property('<table_name>', 'time_to_live_in_seconds', '<non_negative_literal>');
        ```

        -   time\_to\_live\_in\_seconds：表数据的生存时间，单位为秒，必须是非负数字类型，整数或浮点数均可。
        -   可以在事务之外单独使用，表示修改表数据生存时间。
        -   使用示例

            ```
            begin;
            create table tbl (a int not null, b text not null);
            call set_table_property('tbl', 'time_to_live_in_seconds', '3.14159');
            commit;
            -------------------------------------------------------------
            begin;
            create table tbl (a int not null, b time not null);
            call set_table_property('tbl', 'time_to_live_in_seconds', '86400');
            commit;
            ```

            **说明：** 表数据的TTL并不是精确的时间，当超过设置的TTL后，系统会在某一个时间自动删除表数据，所以业务逻辑不能强依赖TTL。若是想精确的删除表数据，可以使用HoloStudio数据开发，进行调度任务配置来删除数据。

    7.  **distribution\_key**

        ```
        call set_table_property('<table_name>', 'distribution_key', '[columnName[,...]]');
        ```

        -   distribution\_key：指定了数据库表分布策略。
        -   columnName部分如设置单值，不要有多余空格。如设置多值，则以逗号分隔，同样不要有多余的空格。
        -   distribution\_key指定的列可以为null。
        -   Hologres中，数据库表默认为随机分布形式。数据将被随机分配到各个shard上。如果制定了分布列，数据将按照指定列，将数据shuffle到各个shard，同样的数值肯定会在同样的shard中。当以分布列做过滤条件时，Hologres可以直接筛选出数据相关的shard进行扫描。当以分布列做join条件时，Hologres不需要再次将数据shuffle到其他计算节点，直接在本节点join本节点数据即可，可以大大提高执行效率。
        -   使用示例

            ```
            begin;
            create table tbl (a int not null, b text not null);
            call set_table_property('tbl', 'distribution_key', 'a');
            commit;
            
            begin;
            create table tbl (a int not null, b text not null);
            call set_table_property('tbl', 'distribution_key', 'a,b');
            commit;
            ```


## 增加注释

Hologres支持给表、外表、列等增加注释（comment）的功能。

增加注释的使用示例如下：

```
-- 给表增加注释
COMMENT ON TABLE table_name IS 'my comments on table table_name.';

-- 给列增加注释
COMMENT ON COLUMN table_name.col1 IS 'This my first col1';

-- 给外部表增加注释
COMMENT ON FOREIGN TABLE foreign_table IS ' comments on my foreign table';
```

更多关于注释的用法请参见[PostgreSQL comment](https://www.postgresql.org/docs/11/sql-comment.html)。

