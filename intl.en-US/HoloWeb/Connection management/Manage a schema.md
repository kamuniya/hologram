---
keyword: [HoloWeb, schema]
---

# Manage a schema

This topic describes how to use HoloWeb to create, edit, and delete a schema in Hologres.

-   An Alibaba Cloud account is registered.
-   Real-name verification is completed.
-   A Hologres instance is purchased. For more information, see [t1877414.md\#](/intl.en-US/Preparations/Purchase a Hologres instance.md).
-   The purchased Hologres instance is connected to HoloWeb. For more information, see [Manage a connection](/intl.en-US/HoloWeb/Connection management/Manage a connection.md).

## Create a schema

1.  Log on to the [HoloWeb console](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F) by using your Alibaba Cloud account.

2.  On the **Connection Management** page, click the **Mode** tab.

    You can also open the New mode dialog box by performing the following steps: In the left-side navigation pane, click **My connection**. All the configured connections appear. Click the target connection and then click Database. Right-click the target database and select New mode, or click the target database, right-click **Schema**, and then select **New mode**.

3.  In the New mode dialog box, set the **Connection name**, **Database name**, and **Pattern name** parameters, and click **OK**.

    ![Create a schema](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5699438951/p132147.png)


## Edit a schema

1.  In the left-side navigation pane, click **My connection**. All the configured connections appear.

2.  Click the target connection and then click **Database**. All the databases created in the connected Hologres instance appear.

3.  Click the target database and then click **Schema**. All the available schemas appear.

    **Note:** After a database is created, a schema named **public** is automatically created.

4.  Right-click the target schema and select **Edit mode**.

5.  In the Edit mode dialog box, change the value of **Pattern name**.

    ![Edit a schema](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5699438951/p132174.png)

    **Note:** When you modify the settings of a schema, you can only change the value of **Pattern name**.


## Delete a schema

1.  In the left-side navigation pane, click **My connection**. All the configured connections appear.

2.  Click the target connection and then click **Database**. All the databases created in the connected Hologres instance appear.

3.  Click the target database and then click **Schema**. All the available schemas appear.

4.  Right-click the target schema and select **Delete mode**.


