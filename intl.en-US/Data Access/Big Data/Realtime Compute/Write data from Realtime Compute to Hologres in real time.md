---
keyword: [write data to Hologres in real time, Realtime Compute]
---

# Write data from Realtime Compute to Hologres in real time

This topic describes how to use the HoloHub API to write data from Realtime Compute to Hologres in real time.

## Prerequisites

-   A Hologres instance is purchased. A development tool is connected to the instance. For more information, see [Use Hologres from the PostgreSQL client](/intl.en-US/Quick Start/Use Hologres from the PostgreSQL client.md).
-   Realtime Compute is activated. For more information, see [Activate Realtime Compute and create a project](/intl.en-US/Exclusive Mode/Preparation/Activate Realtime Compute and create a project.md).

**Note:**

-   Make sure that you activate the Realtime Compute and Hologres services in the same region. Otherwise, the services cannot access each other.
-   Realtime Compute earlier than V3.6 does not support Hologres connectors. To write data to Hologres in real time, reference a JAR package in Realtime Compute. You can [submit a ticket](https://workorder-intl.console.aliyun.com/) or join the Hologres DingTalk group 32314975 to obtain the JAR package.
-   If the Hologres table that is used to receive data contains a primary key, data written to the table is sorted by the primary key by default.

## Procedure

1.  Create a table in Hologres.

    Create a table in Hologres to receive data. The following SQL statement creates a table named blink\_test:

    ```
    create table blink_test(
      a int primary key,
      b text,
      c text
    );
    ```

2.  Create a Realtime Compute job.

    To create a Realtime Compute job for calling the HoloHub API to write data to Hologres in real time, perform the following steps:

    1.  Obtain the endpoint of the HoloHub API.

        Run the following command to query the Virtual Private Cloud \(VPC\) endpoint of the HoloHub API in a Hologres instance of the commercial edition:

        ```
        show hg_datahub_endpoints;
        ```

    2.  Create a Realtime Compute job.

        Log on to the [Realtime Compute console](https://account.alibabacloud.com/login/login.htm?oauth_callback=http://stream-ap-southeast-3.console.aliyun.com/) and create a job. For example, you can use the following SQL statements as the job code:

        ```
        create table randomSource (a int, b VARCHAR, c VARCHAR, d DOUBLE, e BIGINT) with (type = 'random');
        
        create table test (
          a int,
          b VARCHAR,
          c VARCHAR,
          PRIMARY KEY (a)
        ) with (
          type = 'hologres',
          `endpoint` = '$ip:$port', // The VPC endpoint and port number that are used to call the HoloHub API.
          `username` = 'AccessKey ID of the current Alibaba Cloud account',
          `password` = 'AccessKey secret of the current Alibaba Cloud account',
          `dbName` = 'Name of the Hologres database to be connected',
          `tableName` = 'blink_test', // The name of the Hologres table that is used to receive data.
          `batchSize` = '100'
        );
        
        insert
          into test
        select
          a,b,c
        from
          randomSource;
        ```

        The following table describes the WITH parameters.

        |Parameter|Example|Description|
        |---------|-------|-----------|
        |type|hologres|The type of the source table. **Note:** For Realtime Compute V3.6 and later versions, set the value to hologres. For Realtime Compute earlier than V3.6, set the value to custom. |
        |endpoint|demo-hh-cn-hangzhou-vpc.hologres.aliyuncs.com:80|The VPC endpoint and port number that are used to call the HoloHub API.|
        |username|xxxxm3FMWaxxxx|The AccessKey ID of the current Alibaba Cloud account.You can obtain the AccessKey ID in the [User Management](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak) console. |
        |password|xxxxm355fffaxxxx|The AccessKey secret of the current Alibaba Cloud account.You can obtain the AccessKey secret in the [User Management](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak) console. |
        |dbName|Holodb|The name of the Hologres database to be connected.|
        |tableName|blink\_test|The name of the Hologres table that is used to receive data.|

        **Note:** Realtime Compute earlier than V3.6 does not support Hologres connectors. To write data to Hologres in real time, reference a JAR package in Realtime Compute and modify the configuration of the Flink sink. You can [submit a ticket](https://workorder-intl.console.aliyun.com/) or join the Hologres DingTalk group 32314975 to obtain the JAR package.

        -   Set type to custom.
        -   Add `tableFactoryClass = 'com.alibaba.blink.connectors.hologres.table.factory.HologresTableFactory'` after the type parameter.
    3.  Publish the job.

        After you compile the job code, click **Syntax Check** at the top of the job editor. If the **Succeeded** message appears, the code syntax is correct.

        Click **Save** and then **Publish**. In the dialog box that appears, set parameters for committing the job to the production environment as needed.

        -   Configure initial resources.

            ![4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5908158951/p84223.png)

        -   Configure resource settings.

            ![5](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6908158951/p84224.png)

    4.  Start the job.

        After the job is committed to the production environment, manually start the job.

        Click **Administration** at the top of the job editor. On the page that appears, find the job that you want to start and click **Start** in the upper-right corner.

        ![6](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6908158951/p84227.png)

3.  Query the written data in Hologres in real time.

    You can query data written to the Hologres table created in Step 1. You can execute an SQL statement similar to the following example to query the written data:

    ```
    select * from blink_test;
    ```


## Mapping between data types

For more information about the mappings between the data types in Realtime Compute and Hologres, see [Data types](/intl.en-US/Hologres SQL/Data types.md).

