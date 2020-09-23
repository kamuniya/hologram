---
keyword: [Hologres, new user, tutorial]
---

# Use HoloStudio of DataWorks

DataWorks provides HoloStudio for you to connect to Hologres to perform interactive analytics. Before you use HoloStudio, make sure that you have activated Hologres and DataWorks and bound a Hologres instance to a DataWorks workspace. This topic describes how to purchase a Hologres instance, create a DataWorks workspace, and then bind the Hologres instance to the DataWorks workspace for data development.

-   An Alibaba Cloud account is registered.
-   Real-name verification is completed.

The following Alibaba Cloud services are used in this tutorial:

-   [DataWorks](https://www.alibabacloud.com/product/ide)
-   Hologres

## Purchase a Hologres instance

**Note:** If you have purchased a Hologres instance, skip this step and directly create a workspace in DataWorks.

1.  Go to the [Alibaba Cloud official website](https://www.alibabacloud.com/), click **Log In** in the upper-right corner, and then enter your account name and password.

2.  Move the pointer over **Products** in the top navigation bar and choose **Analytics** \> **Data Computing** \> Hologres \(Interactive Analytics\) to go to the product landing page of Hologres.

3.  Click **Buy Now**.

4.  On the buy page of Hologres, click the **Subscription** tab. On the **Subscription** tab, select a region and a computing resource specification, enter an instance name, and then click **Buy Now**.


## Create a database in the Hologres console

After you purchase a Hologres instance, a database named postgres is created by default. This database is allocated with a few resources and only used for management purposes. You can create a database based on your business requirements.

1.  Log on to the [Hologres console](https://hologram.console.aliyun.com/#/instance) and click **Instances** in the left-side navigation pane. On the **Hologres Instances** page, click the name of the target Hologres instance to go to the details page.

2.  Click **Databases**. On the page that appears, click **Create Database**. In the Create Database dialog box, set **Database Name**, select Enable or Disable for **SPM**, and then click **OK**.


## Activate DataWorks

1.  Go to the [Alibaba Cloud official website](https://www.alibabacloud.com/), click **Log In** in the upper-right corner, and then enter your account name and password.

2.  Move the pointer over **Products** in the top navigation bar and choose **Analytics** \> **Data Development** \> DataWorks to go to the product landing page of DataWorks.

3.  Click **Buy Now**.

4.  On the buy page of DataWorks, select a region, an edition, and a duration. Read and agree to the service agreement, and click **Confirm Order and Pay**.

    **Note:** Make sure that you activate the DataWorks and Hologres services in the same region.


## Create a DataWorks workspace

**Note:**

-   You can bind a Hologres instance to an existing DataWorks workspace.
-   To bind a Hologres instance to a DataWorks workspace, make sure that they reside in the same region.

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) by using your Alibaba Cloud account.

2.  On the **Overview** page, click **Create Workspace** in the **Shortcuts** section on the right.

    Alternatively, click Workspaces in the left-side navigation pane. On the Workspaces page, select a region as required and click **Create Workspace**.

3.  In the Create Workspace pane, set parameters in the **Basic Settings** step and click **Next**.

4.  In the **Select Engines and Services** step, select Hologres and click **Next**.

    DataWorks is now available as a commercial service. To create a DataWorks workspace in a region, activate DataWorks in the region first. By default, the following services are selected when you create a workspace: **Data Integration**, **Data Analytics**, **Operation Center**, and **Data Quality**.

5.  In the **Engine Details** step, set parameters for the Hologres engine.

    |Parameter|Description|
    |---------|-----------|
    |Instance display name|The display name of the computing engine instance.|
    |Access identity|Specifies whether to use an Alibaba Cloud account or an authorized Resource Access Management \(RAM\) user to access the Hologres instance.|
    |Hologres instance name|The Hologres instance to be bound to the DataWorks workspace. Select a purchased instance from the drop-down list. **Note:** Make sure that the Hologres instance resides in the same region as the current DataWorks workspace. |
    |Database name|The name of the database to be bound to the workspace.|

6.  After the preceding configuration is complete, click **Create Workspace**.

    After the workspace is created, you can view information about the workspace on the Workspaces page.


## Develop data in HoloStudio

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) with your Alibaba Cloud account.

2.  On the **Workspaces** page, click the name of the target workspace. On the page that appears, click **DataStudio** to enter DataStudio.

3.  In DataStudio, click ![data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1196280061/p113897.png) in the upper-left corner and choose **All Products** \> **HoloStudio** to go to **HoloStudio**.

4.  In HoloStudio, Click the **PG Management** icon in the left-side navigation pane. On the page that appears, click the **Refresh** icon to display the Hologres database bound to the workspace.


The environment configuration is complete and you can develop data in HoloStudio. For more information, see [t1884764.dita\#concept\_1893543](/intl.en-US/HoloStudio/Data Analytics/Overview.md).

