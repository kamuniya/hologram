---
keyword: [connector, real-time write, Hologres, Flink]
---

# Use a connector to write data from Flink to Hologres

This topic describes how to use a connector to write data from Flink sinks, sources, and lookup tables to Hologres.

## Prerequisites

-   A Hologres instance is purchased. A development tool is connected to the instance. For more information, see [Use Hologres from the PostgreSQL client](/intl.en-US/Quick Start/Use Hologres from the PostgreSQL client.md).
-   A Flink cluster is available for you to commit tasks.
-   To write data from a Flink sink, source, or lookup table to Hologres in real time, reference a JAR package in Flink. You can [submit a ticket](https://workorder-intl.console.aliyun.com/) or join the Hologres DingTalk group 32314975 to obtain the JAR package.

## Write data from a Flink sink to Hologres

To create a Flink sink and write data from the sink to Hologres, execute a statement similar to the following example:

```
CREATE TABLE mysource(
  name varchar,
  age BIGINT,
  birthday BIGINT
) WITH (
  'connector.type'='hologres',
  'connector.database'='Name of the Hologres database to be connected',
  'connector.table'='Name of the Hologres table used to receive data',
  'connector.username'='AccessKey ID of the account used to connect to Hologres',
  'connector.password'='AccessKey secret of the account used to connect to Hologres',
  'connector.endpoint'='<ip>:<port>'// The endpoint and port number used to connect to Hologres.
);
```

The following table describes the parameters in the statement.

|Parameter|Description|Required|
|---------|-----------|--------|
|type|The type of the sink.Set the value to **hologres**.

|Yes|
|database|The name of the Hologres database to be connected.|Yes|
|table|The name of the Hologres table that is used to receive data.|Yes|
|username|The AccessKey ID of the account that is used to connect to Hologres.You can obtain the AccessKey ID in the [User Management](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak) console.

|Yes|
|password|The AccessKey secret of the account that is used to connect to Hologres.You can obtain the AccessKey secret in the [User Management](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak) console.

|Yes|
|endpoint|The endpoint used to connect to Hologres.You can view the endpoint of the Hologres instance on the **Configurations** tab of the instance details page in the [Hologres console](https://hologram.console.aliyun.com/#/instance).

|Yes|
|bulkload|Specifies whether to write multiple data entries to Hologres at a time.Default value: false.

|No|
|field\_delimiter|The delimiter used to separate data entries written to Hologres.Do not insert delimiters in data. This parameter takes effect only when the bulkload parameter is set to true.

Default value: \\u0002.

|No|
|upsert\_type|The semantics that is used to write data to Hologres in a continuous manner.Default value: insertorignore.

|No|
|partition\_router|Specifies whether to route data of various partitions to the corresponding child partitioned tables.Default value: false.

|No|

You can use the streaming sinking semantics or batch sinking semantics to write data from a Flink sink to Hologres.

-   Streaming sinking semantics

    If you want to write data from a Flink sink to Hologres in a continuous manner and query the data written to Hologres in real time, use the streaming sinking semantics. We recommend that you use the streaming sinking semantics to write streaming data from a Flink sink to Hologres in real time.

    You can use the exactly-once or at-least-once semantic based on the configurations of the sink and attributes of the Hologres table.

    -   **Exactly-once**: The system processes data or events only once, no matter whether a fault occurs.
    -   **At-least-once**: If data or an event is lost before a process is completed, the system transfers all data and events anew. In this case, data or events may be processed multiple times. If the first retry succeeds, no other retries are required.
    When you use streaming sinking semantics in Flink sinks, pay attention to the following points:

    -   If the destination Hologres table has a primary key, you can ensure the idempotence based on the primary key conflicts to implement the **exactly-once** semantics.

        When data entries with the same primary key are written to Hologres, you can set mutationType to one of the following values to ensure that each data entry is written exactly once to Hologres:

        -   **insertorignore**: Retain each data entry written to Hologres for the first time and ignore all data entries with the same primary key value written to Hologres later. This is the default value.
        -   **insertorreplace**: Update the existing data entry as a whole.
        -   **insertorupdate**: Update the specified fields of the existing data entry.
    -   If the destination Hologres table contains no primary key, implement the **at-least-once** semantics.
    By default, you can only import data from a Flink sink to a non-partitioned table or a child partitioned table in Hologres. If you import data to a parent partitioned table, no error message is returned but you cannot query the written data in Hologres.

    To route data in a parent partitioned table to the corresponding child partitioned tables, set the `partitionRouter` parameter to true. For example, you can use the following statement to create a Flink sink, write data to a parent partitioned table, and then route data from the parent partitioned table to the corresponding child partitioned tables:

    ```
    CREATE TABLE mysource(
      name varchar,
      age BIGINT,
      birthday BIGINT
    ) WITH (
      'connector.type'='hologres',
      'connector.database'='Name of the Hologres database to be connected',
      'connector.table'='Name of the parent partitioned table in Hologres used to receive data',
      'connector.username'='AccessKey ID of the account used to connect to Hologres',
      'connector.password'='AccessKey secret of the account used to connect to Hologres',
      'connector.endpoint'='<ip>:<port>',// The endpoint and port number used to connect to Hologres.
      'connector.partitionRouter'='true',
    );
    ```

    **Note:**

    -   Set the tableName parameter to the name of the parent partitioned table.
    -   Hologres does not automatically create child partitioned tables when it receives data from Flink. You must create child partitioned tables as needed in advance.
-   Batch sinking semantics

    If you want to write a large amount of data entries from a Flink sink to Hologres at a time, use the batch sinking semantics. You can query the data only after all the data is written to Hologres. The data writing process runs as a transaction. If the transaction succeeds, all the data is written to Hologres exactly once.

    The batch sinking semantics improves the high-throughput performance. We recommend that you use the batch sinking semantics when you process multiple data entries in Flink at a time, such as data migration and data backfilling.

    **Note:** To use the batch sinking semantics to write data from a Flink sink to a partitioned table in Hologres, you can only write the data to a child partitioned table.


## Write data from a Flink source to Hologres

You can use the batch reading semantics to read data from a Flink source.

When you use the batch reading semantics to read data from a Flink source, all the snapshot data of the source is read. The data reading process runs as a transaction. If the transaction fails, the system reads the data anew from the snapshot at the latest time point.

We recommend that you use the batch reading semantics when you read a large amount of data entries from a Flink source at a time.

**Note:**

-   Flink sources support the projection pushdown rule, meaning that you can read data from the specified columns in a Flink source.
-   Each parallel instance of Flink can read one or more partitions of the source. We recommend that the number of the parallel instances be no more than that of the partitions.

To create a Flink source and write data from the source to Hologres, execute a statement similar to the following example:

```
CREATE TABLE mysource(
  name varchar,
  age BIGINT,
  birthday BIGINT
) WITH (
  'connector.type'='hologres',
  'connector.database'='Name of the Hologres database to be connected',
  'connector.table'='Name of the Hologres table used to receive data',
  'connector.username'='AccessKey ID of the account used to connect to Hologres',
  'connector.password'='AccessKey secret of the account used to connect to Hologres',
  'connector.endpoint'='<ip>:<port>'// The endpoint and port number used to connect to Hologres.
);
```

The following table describes the parameters in the statement.

|Parameter|Description|Required|
|---------|-----------|--------|
|type|The type of the source.Set the value to **hologres**.

|Yes|
|database|The name of the Hologres database to be connected.|Yes|
|table|The name of the Hologres table that is used to receive data.|Yes|
|username|The AccessKey ID of the account that is used to connect to Hologres.You can obtain the AccessKey ID in the [User Management](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak) console.

|Yes|
|password|The AccessKey secret of the account that is used to connect to Hologres.You can obtain the AccessKey secret in the [User Management](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak) console.

|Yes|
|endpoint|The endpoint used to connect to Hologres.You can view the endpoint of the Hologres instance on the **Configurations** tab of the instance details page in the [Hologres console](https://hologram.console.aliyun.com/#/instance).

|Yes|
|field\_delimiter|The delimiter used to separate data entries written to Hologres.Do not insert delimiters in data.

Default value: \\u0002.

|No|

## Write data from a Flink lookup table to Hologres

You can use a lookup table to join the data processed by Flink with the existing data in Hologres. You can create temporary tables and functions in Hologres to connect to lookup tables in Flink.

**Note:**

-   Lookup tables in Flink do not support the async mode.
-   Lookup tables in Flink do not support caching.
-   If you need help, [submit a ticket](https://workorder-intl.console.aliyun.com/) or contact the technical support of Flink or Hologres.

To create a Flink lookup table and write data from the lookup table to Hologres, execute a statement similar to the following example:

```
CREATE TABLE mysource(
  ...
) WITH (
  'connector.type'='hologres',
  'connector.database'='Name of the Hologres database to be connected',
  'connector.table'='Name of the Hologres table used to receive data',
  'connector.username'='AccessKey ID of the account used to connect to Hologres',
  'connector.password'='AccessKey secret of the account used to connect to Hologres',
  'connector.endpoint'='<ip>:<port>'// The endpoint and port number used to connect to Hologres.
);
```

Examples

```
// register hologreslookup as udf
SELECT * FROM source, LATERAL TABLE(hologreslookup(a, b))

// register hologreslookup as lookup table source
SELECT x, a, b, c FROM src 
    JOIN hologreslookup FOR SYSTEM_TIME AS OF src.proc as h ON src.x = h.a AND src.y = h.b
```

## Appendix: Data types in Flink and Hologres

The following table lists the mappings between data types in Flink and Hologres.

|Flink|Hologres|
|-----|--------|
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

**Note:** Hologres cannot convert Flink data of the data types that are not included in the preceding table.

For more information about the data types supported by Hologres, see [Data types](/intl.en-US/Hologres SQL/Data types.md).

