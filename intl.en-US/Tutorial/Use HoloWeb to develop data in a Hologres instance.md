---
keyword: [Hologres, HoloWeb, tutorial]
---

# Use HoloWeb to develop data in a Hologres instance

Hologres provides HoloWeb for data development. Before you use HoloWeb, purchase a Hologres instance and connect it to HoloWeb. This topic describes how to purchase a Hologres instance, connect an Hologres instance to HoloWeb, and then use HoloWeb to develop data.

-   An Alibaba Cloud account is registered.
-   Real-name verification is completed.

## Purchase a Hologres instance

**Note:** If you have purchased a Hologres instance, skip this step and directly connect the Hologres instance to HoloWeb.

1.  Go to the [Alibaba Cloud official website](https://www.alibabacloud.com/), click **Log In** in the upper-right corner, and then enter your account name and password.

2.  Move the pointer over **Products** in the top navigation bar and choose **Analytics** \> **Data Computing** \> Hologres \(Interactive Analytics\) to go to the product landing page of Hologres.

3.  Click **Buy Now**.

4.  On the buy page of Hologres, click the **Subscription** tab. On the **Subscription** tab, select a region and a computing resource specification, enter an instance name, and then click **Buy Now**.


## Create a database in the Hologres console

After you purchase a Hologres instance, a database named postgres is created by default.

This database is allocated with a few resources and only used for management purposes. You can create a database based on your business requirements.

1.  Log on to the [Hologres console](https://hologram.console.aliyun.com/#/instance) and click **Instances** in the left-side navigation pane. On the **Hologres Instances** page, click the name of the target Hologres instance to go to the details page.

2.  Click **Databases**. On the page that appears, click **Create Database**. In the Create Database dialog box, set **Database Name**, select Enable or Disable for **SPM**, and then click **OK**.


## Create a connection in HoloWeb

1.  Log on to [HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F) by using your Alibaba Cloud account.

2.  Click **Connection Management** in the top navigation bar. On the page that appears, click **Data Connection**.

3.  In the **New Connection** dialog box, set parameters as required and click **OK**. After the connection is created, click **My connection** on the **Connection Management** page to view information about the connected database.

    |Parameter|Description|
    |---------|-----------|
    |Connection name|The name of the connection. Enter a name as required.|
    |Connection description|The descriptions of the connection.|
    |Host|The public endpoint of the Hologres instance.|
    |Port|The public port number of the Hologres instance.|
    |Initialize database|The name of the database to be connected to.|
    |Username|The AccessKey ID of the current Alibaba Cloud account.|
    |Password|The AccessKey secret of the current Alibaba Cloud account.|
    |Test connectivity|Check whether the data connection is successful.|

    **Note:**

    -   On the details page of a Hologres instance in the [Hologres console](https://hologram.console.aliyun.com/#/instance), you can view the public endpoint and port number of the instance on the **Configurations** tab and view databases in the instance on the **Databases** tab.
    -   You can obtain the AccessKey ID and AccessKey secret in the [User Management console](https://usercenter.console.aliyun.com/?spm=5176.2020520153.nav-right.dak.3bcf415dCWGUBj#/manage/ak).

## Create a database in HoloWeb

1.  Log on to [HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F) by using your Alibaba Cloud account.

2.  Click **Connection Management** in the top navigation bar. On the page that appears, click **Database**.

3.  In the New database dialog box, set **Connection name**, **Database name**, and **Permission policy**, and click **OK**.

    **Note:** The database name must be unique.


## Create a schema

1.  Log on to [HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F) by using your Alibaba Cloud account.

2.  Click **Connection Management** in the top navigation bar. On the page that appears, click **Mode**.

3.  In the New mode dialog box, set **Connection name**, **Database name**, and **Pattern name**, and click **OK**.


## Create an internal table

1.  Log on to [HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F) by using your Alibaba Cloud account.

2.  Click **Connection Management** in the top navigation bar. On the page that appears, click **Table**.

3.  On the **New internal table** tab, set parameters as required and click **Submit**.

    ![canshu](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4118280061/p113938.png)


## Create a foreign table

1.  Log on to [HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F) by using your Alibaba Cloud account.

2.  Click **Connection Management** in the top navigation bar. On the page that appears, click **External table**.

3.  On the **New external table** tab, set parameters as required and click **Submit**.

    ![waib](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4118280061/p113940.png)

    **Note:** HoloWeb only supports MaxCompute foreign tables.


## Create an SQL query folder

1.  Log on to [HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F) by using your Alibaba Cloud account.

2.  Click **Query** in the top navigation bar. On the page that appears, click **Folder**.

3.  In the New folder dialog box, set **Folder name** and **Directory**, and click **OK**.


## Create an SQL query

1.  Log on to [HoloWeb](https://account.aliyun.com/login/login.htm?oauth_callback=http%3A%2F%2Fholoweb-cn-shanghai.data.aliyun.com%2F) by using your Alibaba Cloud account.

2.  Click **Query** in the top navigation bar. On the page that appears, click **SQL window**.

3.  In the **New SQL query** dialog box, set parameters as required and click **OK**.

    After an SQL query is created, enter an SQL statement in the editor of the query.

    ![sq](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5118280061/p113942.png)

    **Note:** Before you create an SQL query, make sure that a folder is available for storing the query.


