---
keyword: [HoloWeb, table, preview the table data, preview the DDL statement]
---

# Manage a table

This topic describes how to use HoloWeb to create, edit, and delete an internal table, whose data is stored in Hologres, and preview the data and Data Definition Language \(DDL\) statement of the table.

-   An Alibaba Cloud account is registered.
-   Real-name verification is completed.
-   A Hologres instance is purchased. For more information, see [t1877414.md\#](/intl.en-US/Preparations/Purchase a Hologres instance.md).
-   The purchased Hologres instance is connected to HoloWeb. For more information, see [Manage a connection](/intl.en-US/HoloWeb/Connection management/Manage a connection.md).

## Create a table

1.  Log on to the [HoloWeb console](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F) by using your Alibaba Cloud account.

2.  On the **Connection Management** page, click the **Table** tab.

    You can also go to the table creation tab by performing the following steps:

    1.  In the left-side navigation pane, click **My connection**. All the configured connections appear.

    2.  Click the target connection and then click **Database**. All the databases created in the connected Hologres instance appear.

    3.  Click the target database and then click **Schema**. All the available schemas appear.

    4.  Right-click the target schema and select **New internal table**.

3.  On the **New internal table** tab, set the parameters as required.

    ![New internal table](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4099438951/p132195.png)

    |Category|Parameter|Description|
    |--------|---------|-----------|
    |General information|Connection name|The name of the target connection.|
    |Database|The name of the database where the table is to be created.|
    |Table name|The name of the table.|
    |Description|The description of the table.|
    |Mode|The schema of the table.Default value: **public**. You can also select a custom schema. |
    |Field|Field name|The name of the field to be added to the table.|
    |Data type|The data type of the field.|
    |Primary key|Specifies whether to use the field as the primary key for the table.|
    |Nullable|Specifies whether the field can be left empty.|
    |Array|Specifies whether the field is an ordered array of elements.|
    |Description|The description of the field.|
    |Operation|The operations that you can perform on the field. You can click **Delete** to delete the field and click **Move up** or **Move down** to move the field up or down.|
    |Attribute|Storage mode|The storage mode of table data. Valid values: Column storage and Row deposit.Default value: Column storage. |
    |Life Cycle \(seconds\)|The period for retaining a record that involves no updates. If a record is not updated within that period, the system automatically deletes the record.By default, the system retains records permanently. |
    |Clustered index|The index used for sorting.The type of index determines the order of fields. By using clustered indexes, Hologres can accelerate range and filter queries on index fields. |
    |Segment Key|The fields used for segmenting. Hologres can find the storage location of the corresponding data based on the specified fields.|
    |Dictionary encoding column|The fields based on whose values a dictionary mapping is built.Dictionary encoding can convert string comparisons to numeric comparisons to accelerate queries such as GROUP BY and FILTER.

By default, the system selects all the fields of the **text** type for this parameter. |
    |Bitmap columns|The fields on which bit codes are built.Hologres can find the target records that meet the query conditions based on the specified fields.

By default, the system selects all the fields of the **text** type for this parameter. |
    |Partition Table|N/A|The partition fields of the table.|

4.  Click **Submit**.


## Edit a table

1.  In the left-side navigation pane, click **My connection**. All the configured connections appear.

2.  Click the target connection and then click **Database**. All the databases created in the connected Hologres instance appear.

3.  Click the target database and then click **Schema**. All the available schemas appear.

4.  Click the target schema and then click **Table**. All the tables of the schema created in the database appear.

5.  Right-click the target table and select **Edit table**.

6.  Click **Add Field**. In the section that appears, you can add a field to the table in visualized mode. Then, you can view the corresponding statements in the **SQL Script** editor.

    ![Edit table](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4099438951/p132198.png)

    **Note:** You cannot delete a submitted field in a table when you edit the table.

7.  Click **Submit**.


## Delete a table

1.  In the left-side navigation pane, click **My connection**. Then, find the target table to delete.

    For more information about how to find the target table, see step 1 to step 4 in [Edit a table](#section_uu3_wol_suh).

2.  Right-click the target table and select **Delete table**.


## Preview the table data

1.  In the left-side navigation pane, click **My connection**. Then, find the target table whose data is to preview.

    For more information about how to find the target table, see step 1 to step 4 in [Edit a table](#section_uu3_wol_suh).

2.  Right-click the target table and select **Data preview**.

    On the tab that appears, preview the table data, as shown in the following figure.

    ![Data preview](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4099438951/p132200.png)


## Preview the DDL statement

1.  In the left-side navigation pane, click **My connection**. Then, find the target table whose DDL statement is to preview.

    For more information about how to find the target table, see step 1 to step 4 in [Edit a table](#section_uu3_wol_suh).

2.  Right-click the target table and select **DDL preview**.

    On the tab that appears, preview the DDL statement of the table, as shown in the following figure.

    ![DDL](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4099438951/p132202.png)


