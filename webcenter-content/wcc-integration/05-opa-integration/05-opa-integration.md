# Integrating WebCenter Content with Oracle Process Automation Using REST APIs

## Introduction

This Lab walks you through a scenario demonstrating how to use Oracle WebCenter Content (WCC) REST APIs to build an automated document-driven workflow in Oracle Process Automation (OPA).

Participants will learn how to programmatically generate and share public document links, receive external submissions, and automatically route them through approval processes—while storing and managing all content centrally in WebCenter Content.

**Estimated Lab Time**: *30 minutes*

### Objectives

* Create an OPA application
* Create node JS proxy server

### Prerequisites

This lab assumes you have:

* Access to WCC
* Access to OPA.

## Task 1: Create a Process Application

1. On the Designer home page (All Applications), click the Create button on top right of the page.
  ![Create New App](images/05-opa-integration-task1-step1.jpg "Create New App")

2. The Create Application side pane opens.

   In the **Title** field, enter *Subscription Application*. Enter a meaningful description in the **Description** field. Leave the Version Tag field as **1.0**

        <copy>Subscription Application</copy>

  ![Create New App](images/05-opa-integration-task1-step2.jpg "Create New App")
3. Click **Create** button.

  ![Create Application Screen](images/05-opa-integration-task1-step3.jpg "Create Application Screen")
4. A message indicates that it’s being created, then shows a link. Click the Open now link in the message.

  ![Create Application Screen](images/05-opa-integration-task1-step4.jpg "Create Application Screen")
5. Create Roles

* From the top of the page, click Add.

* In the Add component pane, expand Roles, and click New.

* In the Title field, enter Subscriber, and click Create.
      ![Create Application Screen](images/05-opa-integration-task1-step5.jpg "Create Application Screen")

* Click the **Open** now link or select the role from the Roles page to open it.

* Let’s assign a user and review permissions for the role. In the **Search by** fields:

      * Leave **Users** selected in the drop-down field.
      * In the **Search** ![Create Application Screen](images/05-opa-integration-task1-step5_1.jpg "Create Application Screen") field, enter the first few characters of the user name you signed in with.
      * Select the user. The user gets listed in the page.

* In the **Application Permission Level** options, leave **Use** selected. This allows your user to start an application request in Workspace.
   ![Create Application Screen](images/05-opa-integration-task1-step5_2.jpg "Create Application Screen")

* Repeat steps 1 - 4 to create the second role, only this time enter its name as Approver in the Title field.
   ![Create Application Screen](images/05-opa-integration-task1-step5_3.jpg "Create Application Screen")

* Repeat step 5 and 6 to assign a user for the Approver role and set permission.

## Task 2: Design a Structured Process

   1. Create Process

      * Click the **Subscription Application 1.0** breadcrumb to go to your application’s main page.*

      * From the top of the page, click **Add**.

      * In the Add component pane, expand **Processes**, and click **Structured**.
         ![Create Application Screen](images/05-opa-integration-task1-step5_4.jpg "Create Application Screen")

      * Enter *RFPProcess* in the **Title** field. Click **Create**. A confirmation message shows that the process was created.

         ![Create RFP Process](images/05-opa-integration-task1-step6_1.jpg "Create RFP Process")

      * Select the process to open it. The structured process editor opens. Start and end elements are already positioned on the flow for you. There are two swim lanes and the BPMN elements palette is on the right side.
         ![Create Application Screen](images/05-opa-integration-task1-step6_2.jpg "Create Application Screen")

      * Select the first swimlane containing the start and end element by clicking the bar on the left of the canvas.
         ![Create Application Screen](images/05-opa-integration-task1-step6_3.jpg "Create Application Screen")

      * Click the edit icon to open the Properties pane. In the Properties pane, select Subscriber in the Role drop-down field.
         ![Create Application Screen](images/05-opa-integration-task1-step6_4.jpg "Create Application Screen")

      * Click on start event and click the edit icon. Select Open Properties. Enter the title “Start RFP bid” in Title field
         ![Process Editor Screen](images/05-opa-integration-task1-step6_5.jpg "Process Editor Screen")

   2. Create Simple Data Objects
      1. Open the RFPPRocess process, and then click Data icon on the right.
         ![Process Editor Screen](images/05-opa-integration-task2-step2_1.png "Process Editor Screen")

      2. In the Data pane, click Create Data Object
         ![Process Editor Screen](images/05-opa-integration-task2-step2_2.png "Process Editor Screen")

      3. In the resulting Create Data Object pane, enter *folderId* in the Name field.

      4. Under Data Type , select Simple.

      5. Click Create.
         ![Process Editor Screen](images/05-opa-integration-task2-step2_3.png "Process Editor Screen")

      6. Similarly repeat steps 1-5 to create below data objects.

      | **Name**                  | **Data Type**   |
      |---------------------------|-----------------|
      | folderId                  | String          |
      | linkId                    | String          |
      | vendorFolderId            | String          |
      | publicLinkforVendorFolder | String          |
      | vendorLinkId              | String          |
      | readOnlyLinkId            | String          |

   3. Create Business Type
      1. Click on Add. Select Business Type
        ![Process Editor Screen](images/05-opa-integration-task2-step3_1.png "Process Editor Screen")

      2. Add Title as *EmailBT* and click Create. Click Open Now once created.
        ![Process Editor Screen](images/05-opa-integration-task2-step3_2.png "Process Editor Screen")

      3. Click Add New to add Attributes. Type *email_Id* in Name field. String in Type and Select “Make it an array”
        ![Process Editor Screen](images/05-opa-integration-task2-step3_3.png "Process Editor Screen")

      4. Now, open the  RFPPRocess process, and click Data. In the Data pane, click Create Data Object
        ![Process Editor Screen](images/05-opa-integration-task2-step3_4.png "Process Editor Screen")

      5. In Name field enter *emailDataObject* and Data Type as Business. Select EmailBT from dropdown.
        ![Process Editor Screen](images/05-opa-integration-task2-step3_5.png "Process Editor Screen")

   4. Create UI
      1. Click Add at the top of the page. In the Add component pane, expand UIs and click Web Form
      
## Task 3: Configure WCC REST

Test Edit

## Acknowledgements

* **Authors-** Tania Tavkar -  Principal Solution Engineer - Cloud Adoption | Oracle WebCenter
* **Contributors-** Senthilkumar Chinnappa, Mandar Tengse , Parikshit Khisty
* **Last Updated By/Date-** Senthilkumar Chinnappa, December 2024
