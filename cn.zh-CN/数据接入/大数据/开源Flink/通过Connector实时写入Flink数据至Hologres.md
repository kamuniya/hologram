---
keyword: [Connector, 实时写入, Hologres, Flink]
---

# 通过Connector实时写入Flink数据至Hologres

本文为您介绍如何使用Connector实时写入实时计算Flink的结果表、源表及维表数据至Hologres。

## 前提条件

-   开通Hologres并连接psql客户端，详情请参见[psql客户端快速入门](/cn.zh-CN/快速入门/psql客户端快速入门.md)。
-   已创建可以提交任务的Flink集群。
-   实时写入Flink结果表、源表及维表数据至Hologres，需要您在Flink中引用JAR文件。您可以[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex?spm=5176.2020520129.console-base-top.dwork-order-1.29d546aee0gsiH)或通过交互式分析Hologres交流群（钉钉群号：32314975）获取。

## 创建Sink结果表

在Flink中创建Hologres Sink结果表的语句如下。

```
CREATE TABLE mysource(
  name varchar,
  age BIGINT,
  birthday BIGINT
) WITH (
  'connector.type'='hologres',
  'connector.database'='Hologres的数据库名称',
  'connector.table'='Hologres用于接收数据的表名称',
  'connector.username'='当前账号的AccessKey ID',
  'connector.password'='当前账号的AccessKey SECRET',
  'connector.endpoint'='<ip>:<port>'//Hologres的网络地址和端口号。
);
```

参数说明如下表所示。

|参数|描述|是否为必选参数|
|--|--|-------|
|type|结果表的类型。 固定取值为**hologres**。

|是|
|database|Hologres的数据库名称。|是|
|table|Hologres用于接收数据的表名称。|是|
|username|当前账号的AccessKey ID。 您可以单击[AccessKey 管理](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak)，获取AccessKey ID。

|是|
|password|当前账号的AccessKey Secret。 在您可以单击[AccessKey 管理](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak)，获取AccessKey Secret。

|是|
|endpoint|Hologres的网络地址。 进入[Hologres管理控制台](https://hologram.console.aliyun.com/#/instance)的实例详情页，从**实例配置**获取网络地址。

|是|
|bulkload|使用Bulkload语义写入数据。 默认为false。

|否|
|field\_delimiter|导入数据时，行数据之间使用的分隔符。 分隔符不可以放于数据中。该参数需要与bulkload配合使用。

 默认为"\\u0002"。

|否|
|upsert\_type|使用流式语义写入数据。 默认为insertorignore。

|否|
|partition\_router|写入数据至分区表。 默认为false。

|否|

Sink结果表支持使用流式（Streaming）语义或Bulkload语义写入数据至Hologres，具体如下：

-   Streaming Sink

    Streaming Sink适用于持续向Hologres写入数据的场景，您可以立即查看写入的数据。在Flink流作业中，建议您使用Streaming Sink实时写入数据至Hologres。

    根据Hologres Sink的配置和Hologres表的属性，流式语义分为以下两种：

    -   **Exactly-once**（仅一次）：即使在发生各种故障的情况下，系统只处理一次数据或事件。
    -   **At-least-once**（至少一次）：如果在系统完全处理之前丢失了数据或事件，则从源头重新传输，因此可以多次处理数据或事件。如果第一次重试成功，则不必进行后续重试。
    在Hologres结果表中使用流式语义，您需要注意以下几点：

    -   如果Hologres表已设置主键，则Hologres Sink通过幂等性（idempotent）使用**Exactly-once**语义。

        当同主键数据出现多次时，您需要设置mutationType参数确定更新结果表的方式，mutationType取值如下：

        -   **insertorignore**（默认值）：保留首次出现的数据，忽略后续所有数据。
        -   **insertorreplace**：使用后续出现的数据整行替换已有数据。
        -   **insertorupdate**：使用后续出现的数据选择性替换已有数据。
    -   除以上情况外，Hologres Sink均使用**At-least-once**语义。
    默认Streaming Sink只能导入数据至具体的Hologres表。如果导入数据至分区表，即使导入成功，在Hologres中也查询不到数据。

    您可以设置``partitionRouter` = 'true'`，自动路由数据至分区表子表，示例语句如下。

    ```
    CREATE TABLE mysource(
      name varchar,
      age BIGINT,
      birthday BIGINT
    ) WITH (
      'connector.type'='hologres',
      'connector.database'='Hologres的数据库名称',
      'connector.table'='Hologres用于接收数据的分区表父表名称',
      'connector.username'='当前账号的AccessKey ID',
      'connector.password'='当前账号的AccessKey Secret',
      'connector.endpoint'='<ip>:<port>', //Hologres的网络地址和端口号。
      'connector.partitionRouter'='true',
    );
    ```

    **说明：**

    -   tableName参数为分区表父表的名称。
    -   写入数据时，系统不会自动创建分区表子表，您需要提前在Hologres中创建接收数据的分区表子表。
-   Bulkload Sink

    Bulkload Sink语义适用于一次性批量并高速导入大量数据。数据全部导入后才可以查看。导入的所有数据放于一个事务（Transaction）中，如果事务运行成功，则所有数据均加载成功。Bulkload Sink使用**Exactly-Once**语义导入数据。

    Bulkload Sink优化了高吞吐性能。建议您在数据迁移或回填等Flink批作业中使用Bulkload Sink。

    **说明：** 如果您需要导入数据至分区表时，Bulkload Sink只支持导入至分区表子表。


## 创建Sink源表

Hologres源表仅支持Bulkread语义。

Bulkread Source用于读取当前Hologres表快照的所有数据，读取的数据存放于一个事务中。如果读取数据失败，则Bulkread Source会重新读取当前时间Hologres表的快照。

在Flink Batch作业中，推荐您使用Bulkread Source。

**说明：**

-   源表支持Projection Pushdown，您可以只读取部分列。
-   Flink的每个并发作业均可以读取一个或多个Hologres Shard，建议您配置Flink的并发数小于等于Hologres 的Shard数。

在Flink中创建Hologres源表的语句如下。

```
CREATE TABLE mysource(
  name varchar,
  age BIGINT,
  birthday BIGINT
) WITH (
  'connector.type'='hologres',
  'connector.database'='Hologres的数据库名称',
  'connector.table'='Hologres用于接收数据的表名称',
  'connector.username'='当前账号的AccessKey ID',
  'connector.password'='当前账号的AccessKey Secret',
  'connector.endpoint'='<ip>:<port>'//Hologres的网络地址和端口号。
);
```

参数说明如下表所示。

|参数|描述|是否为必选参数|
|--|--|-------|
|type|源表的类型。 固定取值为**hologres**。

|是|
|database|Hologres的数据库名称。|是|
|table|Hologres用于接收数据的表名称。|是|
|username|当前账号的AccessKey ID。 您可以单击[AccessKey 管理](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak)，获取AccessKey ID。

|是|
|password|当前账号的AccessKey Secret。 在您可以单击[AccessKey 管理](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak)，获取AccessKey Secret。

|是|
|endpoint|Hologres的网络地址。 进入[Hologres管理控制台](https://hologram.console.aliyun.com/#/instance)的实例详情页，从**实例配置**获取网络地址。

|是|
|field\_delimiter|导入数据时，行数据之间使用的分隔符。 分隔符不可以放于数据中。

 默认为"\\u0002"。

|否|

## 创建Sink维表

维表用于连接Flink处理后的数据与Hologres表中已存在的数据。Hologres Connector提供**temporal table**和**temporal function**作为Flink的维表。

**说明：**

-   Hologres维表暂不支持异步模式。
-   Hologres维表暂不支持缓存数据。
-   如果您有其他需求，请[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex?spm=5176.2020520129.console-base-top.dwork-order-1.29d546aee0gsiH)或联系Flink及Hologres的技术支持。

在Flink中创建Hologres维表的语句如下。

```
CREATE TABLE mysource(
  ...
) WITH (
  'connector.type'='hologres',
  'connector.database'='Hologres的数据库名称',
  'connector.table'='Hologres用于接收数据的表名称',
  'connector.username'='当前账号的AccessKey ID',
  'connector.password'='当前账号的AccessKey Secret',
  'connector.endpoint'='<ip>:<port>'//Hologres的网络地址和端口号。
);
```

示例

```
// register hologreslookup as udf
SELECT * FROM source, LATERAL TABLE(hologreslookup(a, b))

// register hologreslookup as lookup table source
SELECT x, a, b, c FROM src 
    JOIN hologreslookup FOR SYSTEM_TIME AS OF src.proc as h ON src.x = h.a AND src.y = h.b
```

## Flink和Hologres的数据类型映射

Flink和Hologres的数据类型映射如下表所示。

|Flink的数据类型|Hologres的数据类型|
|----------|-------------|
|INT|INT|
|ARRAY<INT\>|INT\[\]|
|BIGINT|BIGINT|
|ARRAY<BIGINT\>|BIGINT\[\]|
|FLOAT|REAL|
|ARRAY<FLOAT\>|REAL\[\]|
|DOUBLE|DOUBLE PRECISION|
|ARRAY<DOUBLE\>|DOUBLE PRECISION\[\]|
|BOOLEAN|BOOLEAN|
|ARRAY<BOOLEAN\>|BOOLEAN\[\]|
|VARCHAR|TEXT|
|ARRAY<VARCHAR\>|TEXT\[\]|
|DECIMAL|NUMERIC|
|DATE|DATE|
|TIMESTAMP|TIMESTAMP WITH TIME ZONE|

**说明：** Hologres暂不支持转换上述映射表中不包含的其他数据类型。

Hologres支持的数据类型请参见[数据类型](/cn.zh-CN/SQL参考/数据类型.md)。

