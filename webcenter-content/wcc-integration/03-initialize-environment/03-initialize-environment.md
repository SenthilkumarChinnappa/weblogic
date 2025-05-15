# Initialize WCC Environment

## Introduction

In this lab, we will review and startup all components required to successfully run this workshop.

**Estimated Lab Time**: *30 minutes*

### Objectives

In this lab, you will

- Initialize the workshop environment
- Validate WCC and APEX Instances
- Configure WCC Environment for the Workshop

### Prerequisites

This lab assumes you have:

- A Paid or LiveLabs Oracle Cloud account
- You have completed:
  - Lab: Prepare Setup ( *Paid Tenants* only)
  - Lab: Setup WCC Marketplace Environment

## Task 1: Validate That WebCenter Content CS URL

1. Open the *web browser* window with *WebCenter Content* homepage url ( ie the **WebCenter Content CS Endpoint URL** noted on the previous **Lab 2 - Setup WCC Marketplace Environment** ), click on the *Login* and Login using the below credentials
    - URL
            ```
            <copy>https://localhost:16200/cs/</copy>
            ```

        > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"http://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0"`)
    - Username
            ```
            <copy>weblogic</copy>
            ```
    - Password
            ```
            <copy>Welcome1</copy>
            ```
    > *Note: In the scenario, where WebCenter Content is configured with IDCS or any other username (other than **weblogic**), use user credentials accordingly*
    ![This image shows the WCC Instance Login Page](./images/webcenter_config_task3_step1.png "WCC Instance Login Page")

2. Confirm successful login.

    ![This image shows the status of the WebCenter Content UI Landing page post successful login](./images/webcenter-post-login.png "WebCenter Content UI Landing page post successful login")

    If successful, the page above is displayed and as a result, your WebCenter Content instance is accessible.

3. If you are still unable to log in or the login page is not functioning after reloading ,  proceed as indicated in the **Appendix 1: Restart UCM Server Instance** to restart the services and try login again

4. After you log in to the WebCenter Content Instance successfully, you can proceed with the next Task.

## Task 2: Validate WebCenter Content Search/Index Engine

1. On the new *web browser* window , Login to the *WebCenter Content* homepage URL as Administator User (eg: weblogic). Details are provided below:
    - **URL**
            ```
            <copy>https://localhost:16200/cs/</copy>
            ```

           > Note : Replace `"https://localhost"` with your **hosturl** ( eg: `"http://wcc-rfpmgmt-livelab.livelabs.oraclevcn.com"` or `"https://192.0.0.0"`)
    - **Username**
            ```
            <copy>weblogic</copy>
            ```
    - **Password**
            ```
            <copy>Welcome1</copy>
            ```
    > *Note: In the scenario, where WebCenter Content is configured with IDCS or any other username (other than **weblogic**), use user credentials (which as Administrator Privileges) accordingly*
    ![This image shows the WCC Instance Login Page](./images/webcenter_config_task3_step1.png "WCC Instance Login Page")

2. Under **Administration** tab, click on **Configuration for \<your\_instance\_name\>** and check for the **Search Engine** & **Index Engine Name**, for the value as **ORACLETEXTSEARCH**

    ![This image shows the WCC Instance Configuration Page](./images/task2_webcenter_configuration_page_ots.png "WCC Instance Configuration Page")

3. If the value is showing as **DATABASE.METADATA**, follow the steps in **Appendix 2: Configure Search and Index Engine to use OracleTextSearch**

 ![This image shows the WCC Instance Configuration Page - Database Metadata](./images/task2_webcenter_configuration_page_db_metadata.png "WCC Instance Configuration Page - Database Metadata")

You may now **proceed to the next lab**.

## Appendix 1: Restart UCM Server Instance

1. Login to the WebCenter Content Weblogic console as administrator user (eg : weblogic)

2. Navigate to **Environment** > **Servers** > **Control** tab and select the checkbox for **UCM Server**(s)

3. click on **Shutdown** > **Force Shutdown**

4. After the Server changes to **SHUTDOWN** state, select the checkbox for **UCM Server**(s), click on **Start** button

## Appendix 2: Configure Search and Index Engine to use OracleTextSearch

To set up and use full-text searching and indexing with OracleTextSearch, follow the below steps;

1. Login to WebCenter Content server as user with Administrator Privilege, Under **Administration** tab, navigate to **Admin Server** > **General Configuration**. In the **Additional Configuration Variables** section list of variables, add the below line and click **Save** button
            ```
            <copy>SearchIndexerEngineName=ORACLETEXTSEARCH</copy>
            ```
    ![This image shows the WCC Instance General Configuration Page](./images/task2_webcenter_configuration_page_ots_1.png "WCC Instance General  Configuration Page")

2. Restart the Content Server instance , using the steps mentioned in **Appendix 1: Restart UCM Server Instance**

3. Login to WebCenter Content server as user with Administrator Privilege, Under **Administration** tab, click on **Configuration for <your_instance_name>** and check for the **Search Engine** & **Index Engine Name**, for the value as **ORACLETEXTSEARCH**

    ![This image shows the WCC Instance Configuration Page](./images/task2_webcenter_configuration_page_ots.png "WCC Instance Configuration Page")

4. If you see an ***Alert*** message for ***Index Collection needs to be Synchronized***, perform the steps mentioned in the **Appendix 3: Re-index collections and document** below

    ![This image shows the WCC Instance homepage with Alert Message for Index collection rebuild](./images/appendix3_webcenter_rebuild_index_message.png "WCC Instance  Homepage with Alert Message for Index collection rebuild")

## Appendix 3: Re-index collections and documents

1. Log in to the Content server as an administrator and click on **Admin Applets** under the Administration tab as shown in the image below.

    ![This image shows how to navigate to Admin Applets](./images/appendix2_reindex_screenshot1.png "Navigate to Admin Applets")

2. Click on **Repository Manager** Applet

    ![Click on the Repository Manager Applet as shown in the image.](./images/appendix2_reindex_screenshot2.png "Click on the Repository Manager Applet")

3. Download and Run the **Repository Manager** Java Applet

    ![Click OK to run the Java Applet](./images/appendix2_reindex_screenshot3_0.png "Run Java Applet")

    ![This image shows the Repository Manager Java Applet](./images/appendix2_reindex_screenshot3_1.png "Repository Manager Applet")

4. On the **Repository Manager** Applet , Click on **Indexer** tab

    ![Click Indexer tab in Repository Manager Java Applet](./images/appendix2_reindex_screenshot4_1.png "Indexer Tab in Repository Manager Applet")

5. Under **Collection Rebuild Cycle** section, Click on **Start** Button, *Uncheck* **Use Fast Rebuild** option , Click **OK** button and wait for the indexing to finish

    ![Click Start button of Collection Rebuild  in Indexer tab of Repository Manager Java Applet](./images/appendix2_reindex_screenshot5_1.png "Start Collection Rebuild  Button in Indexer Tab of Repository Manager Applet")  ![This image shows Index Rebuild Options](./images/appendix2_reindex_screenshot5_2.png "Index Rebuild Options")

    ![This image shows Collection Rebuild Finished in Indexer tab of Repository Manager Java Applet](./images/appendix2_reindex_screenshot5_3.png "Collection Rebuild Completed in Indexer Tab of Repository Manager Applet")

### Learn More

- [Introduction To WebCenter Content](https://docs.oracle.com/en/middleware/webcenter/content/12.2.1.4/index.html)
- [Learn More about Apex](https://apex.oracle.com/en/)
- [WebCenter Content - Configuring the Search Index](https://docs.oracle.com/en/middleware/webcenter/content/12.2.1.4/webcenter-content-admin/configuring-search-index.html#GUID-D8372225-70C9-4A3E-987A-279995879606)

## Acknowledgements

- **Authors-** Senthilkumar Chinnappa, Senior Principal Solution Engineer, Oracle WebCenter Content
- **Contributors-** Senthilkumar Chinnappa, Mandar Tengse , Parikshit Khisty
- **Last Updated By/Date-** Senthilkumar Chinnappa, July 2024
