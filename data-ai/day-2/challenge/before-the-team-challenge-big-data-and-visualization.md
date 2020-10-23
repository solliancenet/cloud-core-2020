# Big data and visualization before the team challenge setup guide

**Contents**

<!-- TOC -->

- [Big data and visualization before the team challenge setup guide](#big-data-and-visualization-before-the-team-challenge-setup-guide)
  - [Requirements](#requirements)
  - [Before the team challenge](#before-the-team-challenge)
    - [Task 1: Provision Azure Databricks](#task-1-provision-azure-databricks)
    - [Task 2: Create Azure Storage account](#task-2-create-azure-storage-account)
    - [Task 3: Create storage container](#task-3-create-storage-container)
    - [Task 4: Provision Azure Data Factory](#task-4-provision-azure-data-factory)
    - [Task 5: Download and install Power BI Desktop](#task-5-download-and-install-power-bi-desktop)

<!-- /TOC -->

## Requirements

1. Microsoft Azure subscription must be pay-as-you-go or MSDN.

    a. Trial subscriptions will not work.

2. If you are not a Service Administrator or Co-administrator for the Azure subscription, or if you are running the lab in a hosted environment, you will need to install [Visual Studio 2019 Community](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** and **Azure development** workloads.

## Before the team challenge

Duration: 30 minutes

In this exercise, you will set up your environment for use in the rest of the hands-on lab. You should follow all the steps provided in the Before the Hands-on Lab section to prepare your environment _before_ attending the hands-on lab.

### Task 1: Provision Azure Databricks

Azure Databricks is an Apache Spark-based analytics platform optimized for Azure. It will be used in this lab to build and train a machine learning model used to predict flight delays.

> **Note**: To view the Azure portal menu, select the menu icon in the upper left-hand corner.

![The Azure portal menu is highlighted.](media/portal-menu.png 'Azure portal menu')

1. In the [Azure Portal](https://portal.azure.com) (https://portal.azure.com), select **+ Create a resource** within the portal menu, then type "Azure Databricks" into the search bar. Select Azure Databricks from the results.

   ![The + Create a resource item is selected from the Azure portal menu, Azure Databricks is entered in the search field, and Azure Databricks is selected from the search results list.](media/create-azure-databricks-resource.png)

2. Select **Create**.

3. Set the following configuration on the Azure Databricks Service creation form:

   - **Subscription**: Select the subscription you are using for this hands-on lab.
  
   - **Resource Group**: Select **Create new** and enter a unique name, such as `hands-on-lab-bigdata`

   - **Workspace name**: Enter a unique name, this is indicated by a green checkmark.
  
   - **Location**: Select a region close to you. **_(If you are using an Azure Pass, select South Central US.)_**

   - **Pricing**: Select **Premium (+ Role-based access controls)**

   ![The Azure Databricks Service creation form is filled out with the values as outlined above.](media/azure-databricks-create-blade.png)

4. Select **Review + Create**.

5. Wait for validation to pass, then select **Create**.

### Task 2: Create Azure Storage account

Create a new Azure Storage account that will be used to store historic and scored flight and weather data sets for the lab.

1. In the [Azure Portal](https://portal.azure.com) (<https://portal.azure.com>), select **+ Create a resource**, then type "storage" into the search bar. Select **Storage account** from the results.

   ![The + Create a resource menu item is selected, storage is entered into the search field, and Storage account is selected from the results list.](media/create-azure-storage-resource.png)

2. Select **Create**.

3. Set the following configuration on the Azure Storage account creation form:

   - **Subscription**: Select the subscription you are using for this hands-on lab.

   - **Resource group**: Select the same resource group you created at the beginning of this lab.

   - **Storage account name**: Enter a unique name, this is indicated by a green checkmark.

   - **Location**: Select the same region you used for Azure Databricks.

   - **Performance**: **Standard**

   - **Account kind**: **BlobStorage**

   - **Replication**: **Read-access geo-redundant storage (RA-GRS)**

   - **Access tier**: **Hot**

   ![The Azure storage account creation form is filled out with values as outlined above.](media/azure-storage-create-blade.png)

4. Select **Review + create**.

5. Wait for validation to pass, then select **Create**.

### Task 3: Create storage container

In this task, you will create a storage container in which you will store your flight and weather data files.

1. From the side menu in the Azure portal, choose **Resource groups**, then enter your resource group name into the filter box, and select it from the list.

2. Next, select your lab Azure Storage account from the list.

   ![The Azure Storage account that you created in the previous task is selected from within your lab resource group.](media/select-azure-storage-account.png)

3. Select **Containers** (1) from the menu. Select **+ Container** (2) on the Containers blade, enter **sparkcontainer** for the name (3), leaving the public access level set to Private. Select **Create** (4) to create the container.

   ![The Containers menu item located in the Blob service section is selected from the menu. The + Container item is selected in the the toolbar. The New container form is populated with the values outlined above.](media/azure-storage-create-container.png)

### Task 4: Provision Azure Data Factory

Create a new Azure Data Factory instance that will be used to orchestrate data transfers for analysis.

1. In the [Azure Portal](https://portal.azure.com) (<https://portal.azure.com>), select **+ Create a resource**, then type "Data Factory" into the search bar. Select **Data Factory** from the results.

   ![The + Create a resource item is selected from the Azure Portal menu, Data Factory is entered into the search textbox, and Data Factory is selected from the results list.](media/create-azure-data-factory.png)

2. Select **Create**.

3. Set the following configuration on the Data Factory creation form:

   - **Name**: Enter a unique name, this is indicated by a green checkmark.

   - **Subscription**: Select the subscription you are using for this hands-on lab.

   - **Resource Group**: Select the same resource group you created at the beginning of this lab.

   - **Version**: Select **V2**

   - **Location**: Select any region close to you.

   - **Enable GIT**: **Unchecked**

   **_Understanding Data Factory Location:_**
   The Data Factory location is where the metadata of the data factory is stored and where the triggering of the pipeline is initiated from. Meanwhile, a data factory can access data stores and compute services in other Azure regions to move data between data stores or process data using compute services. This behavior is realized through the [globally available IR](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=data-factory) to ensure data compliance, efficiency, and reduced network egress costs.

   The IR Location defines the location of its back-end compute, and essentially the location where the data movement, activity dispatching, and SSIS package execution are performed. The IR location can be different from the location of the data factory it belongs to.

   ![The Azure Data Factory creation form is populated with the values as outlined above.](media/azure-data-factory-create-blade.png)

4. Select **Create** to finish and submit.

### Task 5: Download and install Power BI Desktop

Power BI desktop is required to make a connection to your Azure Databricks environment when creating the Power BI dashboard.

1. Download and install [Power BI Desktop](https://powerbi.microsoft.com/desktop/).

You should follow all these steps provided _before_ attending the team challenge.
