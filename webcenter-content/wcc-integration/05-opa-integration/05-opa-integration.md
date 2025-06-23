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
         ![Form Editor Screen](images/05-opa-integration-task2-step4_1.png "Form Editor Screen")

      2. In the Title field, enter *RFPInitiationForm*
         ![Form Editor Screen](images/05-opa-integration-task2-step4_2.png "Form Editor Screen")

      3. Click Create, then click the Open now link.

      4. Drag and drop InputText onto the canvas.
         ![Form Editor Screen](images/05-opa-integration-task2-step4_3.png "Form Editor Screen")

      5. Change its label from InputText to *Project Name*
         ![Form Editor Screen](images/05-opa-integration-task2-step4_4.png "Form Editor Screen")

      6. Change the Name in the properties field to *projectName*
         ![Form Editor Screen](images/05-opa-integration-task2-step4_5.png "Form Editor Screen")

      7. Similarly drag another Input Text control onto the canvas and label it *Project Manager Email*
         ![Form Editor Screen](images/05-opa-integration-task2-step4_6.png "Form Editor Screen")

      8. Change the name in properties window to *projectManagerEmail*
         ![Form Editor Screen](images/05-opa-integration-task2-step4_7.png "Form Editor Screen")

      9. Add another control to the canvas and label it *fParentGUID*
         ![Form Editor Screen](images/05-opa-integration-task2-step4_8.png "Form Editor Screen")

      10. Update its name in properties window to *fParentGUID_form*
         ![Form Editor Screen](images/05-opa-integration-task2-step4_9.png "Form Editor Screen")

      11. Set its default value to root/Parent folder GUID in WCC where you want the folders to be created.
         ![Form Editor Screen](images/05-opa-integration-task2-step4_10.png "Form Editor Screen")

      12. Select Hide to hide this value on the form in the properties pane.
         ![Form Editor Screen](images/05-opa-integration-task2-step4_11.png "Form Editor Screen")

      13. Create another form and name it *VendorInputForm*. Click Open Now.
         ![Form Editor Screen](images/05-opa-integration-task2-step4_12.png "Form Editor Screen")

      14. Drag and drop EmailBT from Type onto the canvas.
         ![Form Editor Screen](images/05-opa-integration-task2-step4_13.png "Form Editor Screen")
         ![Form Editor Screen](images/05-opa-integration-task2-step4_14.png "Form Editor Screen")

      15. Update labels as follows
         ![Form Editor Screen](images/05-opa-integration-task2-step4_15.png "Form Editor Screen")
         ![Form Editor Screen](images/05-opa-integration-task2-step4_15.png "Form Editor Screen")

## Task 3: Configure REST Connectors

   1. Go to RFPProcess application, click Add in the upper right corner of the page.In the Add component pane, expand Connectors and choose REST API.

      ![Form Editor Screen](images/05-opa-integration-task3-step1_1.png "Form Editor Screen")

   2. In the Add component pane, enter the following information:

      a.Title: WebCenterConnector
      b.Identifier Name: Automatically populated to uniquely identify the connector.
      c.Base URL: Enter the base endpoint you want the connector to access in REST calls. By default, Process Automation appends .com to the connector’s name and displays that as the default URL.

      ![Form Editor Screen](images/05-opa-integration-task3-step2_1.png "Form Editor Screen")

   3. Click Create and Open the Connector

      *Add REST Connector Resources and Operations*

      1. In the "Add a Resource" field at the top of the section, enter wcc and click +

         ![Resource Editor Screen](images/05-opa-integration-task3-step3_1.png "Resource Editor Screen")

      2. In the Path field, enter the resource path within the base URL

               <copy>documents/wcc/api/v1.1</copy>

         ![Resource Editor Screen](images/05-opa-integration-task3-step3_2.png "Resource Editor Screen")

      3. In the Operations section, click Add and Select POST HTTP method.

         ![Resource Editor Screen](images/05-opa-integration-task3-step3_3.png "Resource Editor Screen")

      4. Expand the Operation.

      5. Enter createFolder in the Name field.

      6. Enter folders in the path field

         ![Resource Editor Screen](images/05-opa-integration-task3-step3_4.png "Resource Editor Screen")

      7. Click the Request tab. In the Body field Select JSON Sample. In the Type name enter *createFolderRequestType*. Click Next. Keep default value for Schema and click Create.

         ![Resource Editor Screen](images/05-opa-integration-task3-step3_5.png "Resource Editor Screen")

      8. Add below parameters

         | **Name**                  | **Data Type**  |
         |---------------------------|----------------|
         | fParentGUID               | QUERY          |
         | fFolderName               | QUERY          |
         | fSecurityGroup            | QUERY          |

         ![Resource Editor Screen](images/05-opa-integration-task3-step3_6.png "Resource Editor Screen")

      9. Click on Response tab. Create Type. Enter *createFolderResponseType*. In sample add below sample response and Click add.

               <copy>{
                   "success": true,
                   "message": "Request was successfully proxied.",
                   "originalResponse": "",
                   "headers": {
                       "date": "Tue, 26 Nov 2024 12:15:40 GMT",
                       "content-length": "0",
                       "connection": "close",
                       "set-cookie": "X-Oracle-OCI-WCC-CS-Route=0e20ed78dc1647b24d06b41ea03add90faa657cd; Path=/; Secure; HttpOnly",
                       "location": "https://129.159.121.118:16200/documents/wcc/api/v1.1/folders/9E995B22C62A7A3704F7F3FD73F230FA",
                       "x-oracle-dms-ecid": "3e7e3019-349e-4d1e-bc9e-9ec76517bfb3-00021f11",
                       "x-oracle-dms-rid": "0"
                   },
                   "additionalInfo": {
                       "timestamp": "2024-11-26T12:15:40.888Z",
                       "proxyServer": "Proxy server for WCC API"
                   }
               }
               </copy>

          ![Resource Editor Screen](images/05-opa-integration-task3-step3_7.png "Resource Editor Screen")

      10. In the Operations section, click Add and Select GET HTTP method.

          ![Resource Editor Screen](images/05-opa-integration-task3-step3_8.png "Resource Editor Screen")

      11. Expand the Operation.

      12. Enter *getFolderInfo* in the Name field.

      13. Enter *folders/search/items* in the path field

          ![Resource Editor Screen](images/05-opa-integration-task3-step3_9.png "Resource Editor Screen")

      14. Click on the Request tab and add below parameters
         | **Name**        | **Data Type**  |
         |-----------------|----------------|
         | itemType        | QUERY          |
         | q               | QUERY          |
         | limit           | QUERY          |
         | fParentGUID     | QUERY          |
         | orderBy         | QUERY          |

      15. Click on Response tab. Create Type *getFolderInfoResponseType*. In sample add below sample response and Click Create.

               <copy>{
                  "success": true,
                  "message": "Request was successfully proxied.",
                  "originalResponse": {
                     "itemType": "Folder",
                     "q": " fCreator LIKE 'tania.tavkar@oracle.com'   AND   UPPER(fFolderName) LIKE UPPER('postmanJan30')   AND  ((fLibraryType != '2' OR fLibraryType IS NULL))   AND  ((fLibraryType != '3' OR fLibraryType IS NULL))   AND  ((UPPER(fFolderName) != UPPER('Trash') OR (UPPER(fFolderName) IS NULL)))   AND  ((UPPER(fFolderName) != UPPER('_REAL_ITEMS') OR (UPPER(fFolderName) IS NULL)))   AND  ((fIsInTrash != '1' OR fIsInTrash IS NULL)) ",
                     "count": 1,
                     "hasMore": false,
                     "totalCount": 1,
                     "orderBy": "fFolderName:Asc",
                     "offset": 0,
                     "limit": 4,
                     "startRow": 0,
                     "nextRow": 1,
                     "dataSource": "FldFolders",
                     "searchEngineName": "DATABASE.METADATA.FOLDERS",
                     "items": [{
                           "fFolderGUID": "E118E68FED7B13074A41EAF5CD20F8CA",
                           "fParentGUID": "CBE4CEF7869EE474ACDC5FD8ACE4B8CF",
                           "fFolderName": "postmanJan30",
                           "fFolderType": "owner",
                           "fInhibitPropagation": 0,
                           "fPromptForMetadata": 0,
                           "fIsContribution": 1,
                           "fIsInTrash": 0,
                           "fRealItemGUID": null,
                           "fLibraryType": null,
                           "fIsLibrary": 0,
                           "fDocClasses": null,
                           "fTargetGUID": null,
                           "fApplication": "framework",
                           "fOwner": "tania.tavkar@oracle.com",
                           "fCreator": "tania.tavkar@oracle.com",
                           "fLastModifier": "tania.tavkar@oracle.com",
                           "fCreateDate": "2025-01-30 13:26:14Z",
                           "fLastModifiedDate": "2025-01-30 13:26:14Z",
                           "fSecurityGroup": "Public",
                           "fDocAccount": null,
                           "fClbraUserList": null,
                           "fClbraAliasList": null,
                           "fClbraRoleList": null,
                           "fFolderDescription": null,
                           "fChildFoldersCount": 0,
                           "fChildFilesCount": 0,
                           "fFolderSize": 0,
                           "fAllocatedFolderSize": -1,
                           "fAllocatorParentFolderGUID": null,
                           "fApplicationGUID": null,
                           "fIsReadOnly": 0,
                           "fIsACLReadOnlyOnUI": 0,
                           "fDisplayName": "postmanJan30",
                           "fDisplayDescription": null,
                           "path": "/RFP folder/postmanJan30",
                           "pathLocalized": "/RFP folder/postmanJan30"
                     }]
                  },
                  "headers": {
                     "date": "Wed, 05 Feb 2025 11:51:42 GMT",
                     "content-type": "application/json",
                     "content-length": "1651",
                     "connection": "close",
                     "set-cookie": "X-Oracle-OCI-WCC-CS-Route=0e20ed78dc1647b24d06b41ea03add90faa657cd; Path=/; Secure; HttpOnly",
                     "x-oracle-dms-ecid": "3e7e3019-349e-4d1e-bc9e-9ec76517bfb3-001c3e03",
                     "x-oracle-dms-rid": "0"
                  },
                  "additionalInfo": {
                     "timestamp": "2025-02-05T11:51:42.147Z",
                     "proxyServer": "Proxy server for WCC API"
                  }
               }
               </copy>

            ![Resource Editor Screen](images/05-opa-integration-task3-step3_10.png "Resource Editor Screen")

      16. In the Operations section, click Add and Select POST HTTP method.

            ![Resource Editor Screen](images/05-opa-integration-task3-step3_11.png "Resource Editor Screen")

      17. Expand the Operation.
      18. Enter *createPublicLink* in the Name field.
      19. Enter *publiclinks/.by.folder/{fFolderGUID}* in the path field

            ![Resource Editor Screen](images/05-opa-integration-task3-step3_12.png "Resource Editor Screen")

      20. Click on Request Tab and In the Body field Select JSON Sample. In the Type name enter *createPublicLinkRequestType*.

      21. Enter the below code in sample

               <copy>{
                        "dLinkName": "external_review",
                        "dLinkPrivilege": "R"
                     }
               </copy>
      22. Click Next and click Create. *Note* : Parameter fFolderGUID is automatically added.

            ![Resource Editor Screen](images/05-opa-integration-task3-step3_13.png "Resource Editor Screen")

      23. Click on Response Tab and In the Body field Select JSON Sample. In the Type name enter  *createPublicLinkResponseType*. Add below code in the Sample.

               <copy>{
                   "success": true,
                   "message": "Request was successfully proxied.",
                   "originalResponse": "",
                   "headers": {
                       "date": "Tue, 26 Nov 2024 13:36:39 GMT",
                       "content-length": "0",
                       "connection": "close",
                       "set-cookie": "X-Oracle-OCI-WCC-CS-Route=0e20ed78dc1647b24d06b41ea03add90faa657cd; Path=/; Secure; HttpOnly",
                       "location": "https://129.159.121.118:16200/documents/wcc/api/v1.1/publiclinks/LF809DBB638216BA6753E0D802B3D27A1F03616FF0C4",
                       "x-oracle-dms-ecid": "3e7e3019-349e-4d1e-bc9e-9ec76517bfb3-000223b9",
                       "x-oracle-dms-rid": "0"
                   },
                   "additionalInfo": {
                       "timestamp": "2024-11-26T13:36:39.564Z",
                       "proxyServer": "Proxy server for WCC API"
                   }
               }
               </copy>

      24. Click Next and Create.
      25. In the Operations section, click Add and Select GET HTTP method.

            ![Resource Editor Screen](images/05-opa-integration-task3-step3_14.png "Resource Editor Screen")

      26. Expand the Operation.
      27. Enter *getPublickLinkId* in the Name field.
      28. Enter *publiclinks/.by.folder/{fFolderGUID}* in the path field
      29. Parameter fFolderGUID is automatically added.
      30. Add below additional parameters

         | **Name**       | **Data Type**  |
         |----------------|----------------|
         | orderBy        | QUERY          |
         | limit          | QUERY          |

            ![Resource Editor Screen](images/05-opa-integration-task3-step3_15.png "Resource Editor Screen")

      31. Click on Response tab. Create Type *getPublicLinkIdResponseType*. In sample add below sample response 

            <copy>{
                "success": true,
                "message": "Request was successfully proxied.",
                "originalResponse": {
                    "offset": 0,
                    "limit": 20,
                    "count": 2,
                    "orderBy": "dLinkName:Asc",
                    "hasMore": false,
                    "totalResults": 2,
                    "items": [{
                        "dLinkID": "LF420216602B886512911FA842235BD9EDA18E0283D3",
                        "dLinkType": "fFolderGUID",
                        "dLinkName": "external_review",
                        "dLinkDesc": "",
                        "fItemGUID": "46DDB6AE404D45DF0C8DBE3F49DB875E",
                        "fOwner": "tania.tavkar@oracle.com",
                        "dLinkPrivilege": "R",
                        "dAssignedUsers": "@everybody",
                        "dAccessCode": "",
                        "dCreatedDate": "2025-02-04 12:48:30Z",
                        "dLastModifiedDate": "2025-02-04 12:48:30Z",
                        "dExpirationDate": "2025-03-06 12:48:30Z",
                        "dCreator": "tania.tavkar@oracle.com"
                    }, {
                        "dLinkID": "LFD39743E7071DCF0114F0504E74F420D7A9127C8268",
                        "dLinkType": "fFolderGUID",
                        "dLinkName": "newLink",
                        "dLinkDesc": "",
                        "fItemGUID": "46DDB6AE404D45DF0C8DBE3F49DB875E",
                        "fOwner": "tania.tavkar@oracle.com",
                        "dLinkPrivilege": "R",
                        "dAssignedUsers": "@everybody",
                        "dAccessCode": "",
                        "dCreatedDate": "2025-02-05 13:57:12Z",
                        "dLastModifiedDate": "2025-02-05 13:57:12Z",
                        "dExpirationDate": "2025-03-07 13:57:12Z",
                        "dCreator": "tania.tavkar@oracle.com"
                    }]
                },
                "headers": {
                    "date": "Wed, 05 Feb 2025 13:58:26 GMT",
                    "content-type": "application/json",
                    "content-length": "972",
                    "connection": "close",
                    "set-cookie": "X-Oracle-OCI-WCC-CS-Route=0e20ed78dc1647b24d06b41ea03add90faa657cd; Path=/; Secure; HttpOnly",
                    "x-oracle-dms-ecid": "3e7e3019-349e-4d1e-bc9e-9ec76517bfb3-001c488f",
                    "x-oracle-dms-rid": "0"
                },
                "additionalInfo": {
                    "timestamp": "2025-02-05T13:58:26.450Z",
                    "proxyServer": "Proxy server for WCC API"
                }
            }
            </copy>

## Task 4: Complete the Process configuration

   1. Go to the process and click on Start Event.  Open Properties.
   2. Under UI, select form RFPInitiatorForm

      ![Process Editor Screen](images/05-opa-integration-task4-step1_1.png "Process Editor Screen")

   3. Drag a service activity to the canvas. Rename it *createFolder*. Click Open Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step3_1.png "Process Editor Screen")

   4. Complete implementation fields. Select Service, Resource, and Operation entries

      | **Name**       | **Value**            |
      |----------------|----------------------|
      | Service        | *WebCenterConnector* |
      | Resource       | *wcc*                |
      | Operation      | *createFolder*       |

      ![Process Editor Screen](images/05-opa-integration-task4-step4_1.png "Process Editor Screen")

   5. Open Data Association

      ![Process Editor Screen](images/05-opa-integration-task4-step5_1.png "Process Editor Screen")

   6. Create mapping as below:

      ![Process Editor Screen](images/05-opa-integration-task4-step6_1.png "Process Editor Screen")

   7. Drag a service activity to the canvas. Rename it getFolderInfo. Click Open Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step7_1.png "Process Editor Screen")

   8. Complete implementation fields. Select Service, Resource, and Operation entries

      | **Name**       | **Value**            |
      |----------------|----------------------|
      | Service        | *WebCenterConnector* |
      | Resource       | *wcc*                |
      | Operation      | *getFolderInfo*      |

      ![Process Editor Screen](images/05-opa-integration-task4-step8_1.png "Process Editor Screen")

   9. Open Data Association

      ![Process Editor Screen](images/05-opa-integration-task4-step9_1.png "Process Editor Screen")

   10. Create Input mapping as below:

      ![Process Editor Screen](images/05-opa-integration-task4-step10_1.png "Process Editor Screen")

   11. Create Output mapping as below:

      ![Process Editor Screen](images/05-opa-integration-task4-step11_1.png "Process Editor Screen")

   12. Drag a service activity to the canvas. Rename it createPublicLink. Click Open Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step12_1.png "Process Editor Screen")

   13. Complete implementation fields. Select Service, Resource, and Operation entries

      | **Name**       | **Value**            |
      |----------------|----------------------|
      | Service        | *WebCenterConnector* |
      | Resource       | *wcc*                |
      | Operation      | *createPublicLink*   |

      ![Process Editor Screen](images/05-opa-integration-task4-step13_1.png "Process Editor Screen")

   14. Open Data Association and create Input Mapping

      ![Process Editor Screen](images/05-opa-integration-task4-step14_1.png "Process Editor Screen")

   15. Drag a service activity to the canvas. Rename it getLinkId. Click Open Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step15_1.png "Process Editor Screen")

   16. Complete implementation fields. Select Service, Resource, and Operation entries

      | **Name**       | **Value**            |
      |----------------|----------------------|
      | Service        | *WebCenterConnector* |
      | Resource       | *wcc*                |
      | Operation      | *getPublicLinkId*    |

      ![Process Editor Screen](images/05-opa-integration-task4-step16_1.png "Process Editor Screen")

   17. Open Data Association and create Input Mapping as below

      ![Process Editor Screen](images/05-opa-integration-task4-step17_1.png "Process Editor Screen")

   18. Create Output mapping as below . Click Apply.

      ![Process Editor Screen](images/05-opa-integration-task4-step18_1.png "Process Editor Screen")

   19. Drag a service activity to the canvas. Rename it *createReadOnlyLink*. Click Open Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step19_1.png "Process Editor Screen")

   20. Complete implementation fields. Select Service, Resource, and Operation entries

      | **Name**       | **Value**            |
      |----------------|----------------------|
      | Service        | *WebCenterConnector* |
      | Resource       | *wcc*                |
      | Operation      | *createPublicLink*   |

      ![Process Editor Screen](images/05-opa-integration-task4-step20_1.png "Process Editor Screen")

   21. Open Data Association and create Input Mapping. Click Apply.

      ![Process Editor Screen](images/05-opa-integration-task4-step21_1.png "Process Editor Screen")

   22. Drag a service activity to the canvas. Rename it *getReadOnlyLinkId*. Click Open Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step22_1.png "Process Editor Screen")

   23. Complete implementation fields. Select Service, Resource, and Operation entries

      | **Name**       | **Value**            |
      |----------------|----------------------|
      | Service        | *WebCenterConnector* |
      | Resource       | *wcc*                |
      | Operation      | *getReadOnlyLinkId*  |


      ![Process Editor Screen](images/05-opa-integration-task4-step23_1.png "Process Editor Screen")

   24. Open Data Association and create Input Mapping.

      ![Process Editor Screen](images/05-opa-integration-task4-step24_1.png "Process Editor Screen")

   25. Create Output mapping . Click Apply.

      ![Process Editor Screen](images/05-opa-integration-task4-step25_1.png "Process Editor Screen")

   26. Drag a Notify activity onto canvas. Rename it Notify Project Manager. Click Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step26_1.png "Process Editor Screen")

   27. In the To field select Expressions tab.

      ![Process Editor Screen](images/05-opa-integration-task4-step27_1.png "Process Editor Screen")

   28. Choose the project manager email id field from form arguments. Click +.

      ![Process Editor Screen](images/05-opa-integration-task4-step28_1.png "Process Editor Screen")

   29. In the Subject field enter *“Folder has been shared with you”*

   30. In the body add below text and make sure to select **Expression mode**

      <copy>"You can upload your bids in the following location: https://wccpm-wcs.cec.ocp.oc-test.com:16200/cs/idcplg/cs/idcplg?IdcService=REDWOODUI_FOLDER_VIEWER&fFolderGUID="+folderId+"&dLinkID="+linkId
      </copy>

      ![Process Editor Screen](images/05-opa-integration-task4-step30_1.png "Process Editor Screen")

   31. Go to Process. Drag Approve human task onto canvas. Rename it Enter Vendors

      ![Process Editor Screen](images/05-opa-integration-task4-step31_1.png "Process Editor Screen")

   32. Click Properties. Under UI select VendorInputForm

      ![Process Editor Screen](images/05-opa-integration-task4-step32_1.png "Process Editor Screen")

   33. Open Data Association and complete Output mapping as below.

      ![Process Editor Screen](images/05-opa-integration-task4-step33_1.png "Process Editor Screen")

   34. Drag a Subprocess activity on the canvas. Rename it Vendor Folders

      ![Process Editor Screen](images/05-opa-integration-task4-step34_1.png "Process Editor Screen")

   35. Click multi-instance icon.

      ![Process Editor Screen](images/05-opa-integration-task4-step35_1.png "Process Editor Screen")

   36. Complete the entries in the The Multi-instance properties pane.

      ![Process Editor Screen](images/05-opa-integration-task4-step36_1.png "Process Editor Screen")

   37. Now expand the subprocess activity and drag a service activity into the subprocess. Rename it createVendorFolder. Click Open Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step37_1.png "Process Editor Screen")

   38. Complete implementation fields. Select Service, Resource, and Operation entries

      | **Name**       | **Value**            |
      |----------------|----------------------|
      | Service        | *WebCenterConnector* |
      | Resource       | *wcc*                |
      | Operation      | *createFolder*       |


      ![Process Editor Screen](images/05-opa-integration-task4-step38_1.png "Process Editor Screen")

   39. Open Data Association

      ![Process Editor Screen](images/05-opa-integration-task4-step39_1.png "Process Editor Screen")

   40. Create mapping as below . Click Apply.

      ![Process Editor Screen](images/05-opa-integration-task4-step40_1.png "Process Editor Screen")

   41. Drag a service activity into the subprocess. Rename it getFolderInfo. Click Open Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step41_1.png "Process Editor Screen")

   42. Complete implementation fields. Select Service, Resource, and Operation entries

      | **Name**       | **Value**            |
      |----------------|----------------------|
      | Service        | *WebCenterConnector* |
      | Resource       | *wcc*                |
      | Operation      | *getFolderInfo*      |

      ![Process Editor Screen](images/05-opa-integration-task4-step42_1.png "Process Editor Screen")

   43. Open Data Association and Create Input mapping as below. 

      ![Process Editor Screen](images/05-opa-integration-task4-step43_1.png "Process Editor Screen")

   44. Create Output mapping as below

      ![Process Editor Screen](images/05-opa-integration-task4-step44_1.png "Process Editor Screen")

   45. Drag a service activity into subprocess. Rename it createVendorPublicLink. Click Open Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step45_1.png "Process Editor Screen")

   46. Complete implementation fields. Select Service, Resource, and Operation entries

      | **Name**       | **Value**            |
      |----------------|----------------------|
      | Service        | *WebCenterConnector* |
      | Resource       | *wcc*                |
      | Operation      | *createPublicLink*   |

      ![Process Editor Screen](images/05-opa-integration-task4-step46_1.png "Process Editor Screen")

   47.Open Data Association and create Input Mapping. Click Apply.

      ![Process Editor Screen](images/05-opa-integration-task4-step47_1.png "Process Editor Screen")

   48. Drag a service activity into the subprocess. Rename it getVendorLinkId. Click Open Properties.

      ![Process Editor Screen](images/05-opa-integration-task4-step48_1.png "Process Editor Screen")

   49. Complete implementation fields. Select Service, Resource, and Operation entries

      | **Name**       | **Value**            |
      |----------------|----------------------|
      | Service        | *WebCenterConnector* |
      | Resource       | *wcc*                |
      | Operation      | *getPublicLinkId*    |

      ![Process Editor Screen](images/05-opa-integration-task4-step49_1.png "Process Editor Screen")

   50. Open Data Association and create Input Mapping. 

      ![Process Editor Screen](images/05-opa-integration-task4-step50_1.png "Process Editor Screen")

   51. Create Output mapping . Click Apply

      ![Process Editor Screen](images/05-opa-integration-task4-step51_1.png "Process Editor Screen")

   52. Drag a Notify activity into the subprocess. Rename it Notify Vendors. Open Properties

      ![Process Editor Screen](images/05-opa-integration-task4-step52_1.png "Process Editor Screen")

   53. In the To field select Expressions tab.

      ![Process Editor Screen](images/05-opa-integration-task4-step53_1.png "Process Editor Screen")

   54. Select loop.inputDataItem. Click +.

      ![Process Editor Screen](images/05-opa-integration-task4-step54_1.png "Process Editor Screen")

   55. In the Subject field enter “Vendor Folder has been shared with you”
   56. In the body add below text and make sure to select Expression mode

      <copy>"You can still download the original documents from the following location: "
                        + "https://wccpm-wcs.cec.ocp.oc-test.com:16200/cs/idcplg/cs/idcplg?IdcService=REDWOODUI_FOLDER_VIEWER"
                        + "&fFolderGUID=" + folderId
                        + "&dLinkID=" + readOnlyLinkId
                        + "\nYou can upload your bids at the location below: "
                        + "https://wccpm-wcs.cec.ocp.oc-test.com:16200/cs/idcplg/cs/idcplg?IdcService=REDWOODUI_FOLDER_VIEWER"
                        + "&fFolderGUID=" +vendorfolderGUID
                        + "&dLinkID=" + vendorLinkId
      </copy>

         ![Process Editor Screen](images/05-opa-integration-task4-step56_1.png "Process Editor Screen")

   

## Acknowledgements

* **Authors-** Tania Tavkar -  Principal Solution Engineer - Cloud Adoption | Oracle WebCenter
* **Contributors-** Senthilkumar Chinnappa, Mandar Tengse , Parikshit Khisty
* **Last Updated By/Date-** Senthilkumar Chinnappa, December 2024
