# IoT and the Smart City team challenge

**Contents**

<!-- TOC -->

- [IoT and the Smart City team challenge](#iot-and-the-smart-city-team-challenge)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Exercise 1: Set up the IoT Remote Monitoring solution environment](#exercise-1-set-up-the-iot-remote-monitoring-solution-environment)
    - [Help references](#help-references)
    - [Task 1: Provision the Remote Monitoring Solution](#task-1-provision-the-remote-monitoring-solution)
    - [Task 2: Stop running device simulation in the Remote Monitoring Solution](#task-2-stop-running-device-simulation-in-the-remote-monitoring-solution)
  - [Exercise 2: Provision additional Azure services](#exercise-2-provision-additional-azure-services)
    - [Help references](#help-references-1)
    - [Task 1: Create Service Bus queue](#task-1-create-service-bus-queue)
    - [Task 2: Create the Critical Alerts container in Cosmos Db](#task-2-create-the-critical-alerts-container-in-cosmos-db)
    - [Task 3: Review the consumer groups in the IoT Hub](#task-3-review-the-consumer-groups-in-the-iot-hub)
    - [Task 4: Review the Azure Time Series Insights instance](#task-4-review-the-azure-time-series-insights-instance)
    - [Task 5: Provision an Azure Container Registry](#task-5-provision-an-azure-container-registry)
    - [Task 6: Obtain the Storage Account Connection String](#task-6-obtain-the-storage-account-connection-string)
    - [Task 7: Create storage account containers required for the lab](#task-7-create-storage-account-containers-required-for-the-lab)
    - [Task 8: Retrieve Secrets from the Key Vault](#task-8-retrieve-secrets-from-the-key-vault)
  - [Exercise 3: Create bus and traffic light simulated devices, and add alerts and filters](#exercise-3-create-bus-and-traffic-light-simulated-devices-and-add-alerts-and-filters)
    - [Help references](#help-references-2)
    - [Task 1: Configure the Device Simulation projects to run locally](#task-1-configure-the-device-simulation-projects-to-run-locally)
    - [Task 2: Finish configuring the simulated IoT device models and scripts](#task-2-finish-configuring-the-simulated-iot-device-models-and-scripts)
    - [Task 3: Explore the remaining files to understand what is happening](#task-3-explore-the-remaining-files-to-understand-what-is-happening)
    - [Task 4: Configure and run the Storage Adapter project](#task-4-configure-and-run-the-storage-adapter-project)
    - [Task 5: Run the Simulator web service and create a new simulation](#task-5-run-the-simulator-web-service-and-create-a-new-simulation)
    - [Task 6: Create alerts and filters in the monitoring web app](#task-6-create-alerts-and-filters-in-the-monitoring-web-app)
    - [Task 7: Send jobs to IoT devices](#task-7-send-jobs-to-iot-devices)
  - [Exercise 4: Create IoT Edge device and custom modules](#exercise-4-create-iot-edge-device-and-custom-modules)
    - [Help references](#help-references-3)
    - [Task 1: Add a new IoT Edge device](#task-1-add-a-new-iot-edge-device)
    - [Task 2: Provision new Linux virtual machine to run as the IoT Edge device](#task-2-provision-new-linux-virtual-machine-to-run-as-the-iot-edge-device)
    - [Task 3: Create and upload the custom C\# IoT Edge module for vehicle telemetry](#task-3-create-and-upload-the-custom-c-iot-edge-module-for-vehicle-telemetry)
    - [Task 4: Create the Azure Stream Analytics IoT Edge module](#task-4-create-the-azure-stream-analytics-iot-edge-module)
    - [Task 5: Deploy custom modules to IoT Edge device](#task-5-deploy-custom-modules-to-iot-edge-device)
  - [Exercise 5: Create an Azure Function to add critical engine alerts to the Service Bus Queue](#exercise-5-create-an-azure-function-to-add-critical-engine-alerts-to-the-service-bus-queue)
    - [Task 1: Create a new Function App](#task-1-create-a-new-function-app)
    - [Task 2: Add new Function to process messages received by the IoT Hub](#task-2-add-new-function-to-process-messages-received-by-the-iot-hub)
  - [Exercise 6: Run a console app to view critical engine alerts from the Service Bus Queue](#exercise-6-run-a-console-app-to-view-critical-engine-alerts-from-the-service-bus-queue)
    - [Help references](#help-references-4)
    - [Task 1: Retrieve the Service Bus Queue Connection string](#task-1-retrieve-the-service-bus-queue-connection-string)
    - [Task 2: Configure and execute the ReadEngineAlerts solution in Visual Studio 2019](#task-2-configure-and-execute-the-readenginealerts-solution-in-visual-studio-2019)
  - [After the team challenge](#after-the-team-challenge)
    - [Task 1: Deprovision the accelerator through the website](#task-1-deprovision-the-accelerator-through-the-website)

<!-- /TOC -->

## Abstract and learning objectives

In this team challenge, you will build an end-to-end smart city solution. We will begin with deploying the Azure IoT Remote Monitoring Accelerator. This accelerator is meant to serve as an example of the recommendations set forth in the [Azure IoT Reference Architecture](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/iot/). This accelerator may also serve as a customizable foundation for any real-world Remote Monitoring IoT systems. It has been built with the flexibility to define telemetry schemas, device types, groups, rules, and more. The Remote Monitoring accelerator also supports both simulated and real IoT devices - including an IoT Edge devices. The Remote Monitoring Web application displays all IoT data with charts and alerts based on pre-configured rules for each type of IoT device. You will also use this custom web app to configure IoT devices and send control messages to them via the IoT Hub.

In this lab, we will deploy an Edge Device with a custom-built module along with an on-device analytics engine which intelligently filters vehicle telemetry data for anomalies and transmits only this subset of data to IoT Hub thus reducing noise and saving on bandwidth and its associated costs. Non-critical data will be sent to storage on the device and uploaded to the cloud once connectivity is established. All telemetry data will also be stored in Time Series Insights in the cloud and ordered based on the timestamp in the telemetry message. All critical data will also flow through an Azure Function that routes critical alerts to a Service Bus Queue for separate processing and storage.

At the end of this team challenge, you will be better able to build an end-to-end IoT solution that processes and analyzes data both in the field and in the cloud.

## Overview

The IoT and the Smart City team challenge is an exercise that will challenge you to implement an end-to-end scenario using a supplied sample that is based on IoT Hub, IoT Edge devices, Cosmos DB, Time Series Insights, Service Bus, and related Azure services. The team challenge can be implemented on your own, but it is highly recommended to pair up with other members at the lab to model a real-world experience and to allow each member to share their expertise for the overall solution.

## Solution architecture

![The Solution Architecture diagram is described in the text following the diagram.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image2.png 'Solution Architecture diagram')

The solution begins with an **IoT Edge Device** that would be installed on each city bus, which is responsible for reading the vehicle telemetry from the bus, such as speed, fuel level, oil level, engine temperature, etc., generated by **custom C\# module**. A **Stream Analytics module** is loaded on the IoT Edge device to filter the vehicle telemetry data so that only anomalies are sent to **IoT Hub**, all data is also stored locally on the device in the **Edge Storage Module**. The Edge Storage module is configured to upload data to Azure Blob Storage when adequate connectivity is achieved. A GPS IoT device is separately added to the bus to periodically send location and speed data to IoT Hub.

An IoT device is added to various traffic lights to send timing and voltage telemetry. It is registered as a device in IoT Hub, including properties such as its longitude and latitude and serial number. It can receive cloud-to-device messages through IoT Hub, allowing upstream services to send updates like adjusting the timing of its lights. The bus and traffic light devices are simulated using the IoT Remote Monitoring solution device simulator.

The Azure IoT Hub is configured with multiple consumer groups. This allows multiple downstream services to simultaneously read the incoming data.  **Time Series Insights** instance and an Azure Function microservice will read the incoming data. Time Series Insights is used to store the raw time series data and provides advanced filtering, custom ad-hoc queries, and visualizations that can overlay data from several classes of IoT devices. A second input into Time Series Insights provides the path for non-critical vehicle telemetry to be included from BLOB Storage.

An **Azure Function** using a separate consumer group routes filtered bus engine-related critical messages to a **Service Bus Queue**, where another **Azure Function** is triggered and outputs the critical alerts to a new collection in Cosmos DB.

The web app, which is part of the IoT Remote Monitoring solution, uses Device Twin information to display IoT devices on a map, provide real-time charts, and allows you to manage alert rules and device control messages. It is also used to provision new IoT devices and send manual cloud-to-device messages through IoT Hub.

## Requirements

- Microsoft Azure subscription (non-Microsoft subscription) where you are at least a co-administrator.

- **Global Administrator role** for Azure AD within your subscription.

- Local machine or a virtual machine configured with:

>**Note**: Included below is instruction on automating the creation of an Azure VM that includes all of the necessary software to complete the labs. Perform Task 1 only if you do not want to run locally. **It is important to complete these steps the day before the lab**!

  - Visual Studio Code version 1.38.1 or higher

    - <https://code.visualstudio.com/>

  - Azure IoT Edge extension for Visual Studio Code

    - <https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge>

  - C\# for Visual Studio Code (powered by OmniSharp) extension

    - <https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp>

  - Docker Desktop on the same computer that has Visual Studio Code

    - <https://docs.docker.com/engine/installation/>

  - .NET Core 2.2 SDK

    - <https://dotnet.microsoft.com/download>

  - Visual Studio Community 2019 or greater, version 16.2.5 or higher

    - <https://www.visualstudio.com/vs/>

  - Azure development workload for Visual Studio 2019

    - <https://docs.microsoft.com/azure/azure-functions/functions-develop-vs#prerequisites>

  - .NET desktop development workload for Visual Studio 2019

  - ASP.NET and web development workload for Visual Studio 2019

  - Node.js (install using either the 32-bit or 64-bit Windows Installer (.msi) option)

    - <https://nodejs.org/en/download/>

  - Postman app

    - <https://www.getpostman.com/apps>

  - Bash client (such as Git Bash or Bash on Ubuntu for Windows)

    - Instructions for installing the Windows Subsystem for Linux for using Bash: <https://docs.microsoft.com/en-us/windows/wsl/install-win10>

## Exercise 1: Set up the IoT Remote Monitoring solution environment

**Duration:** 30 Minutes

In this exercise, you will take advantage of the 'Remote Monitoring' Microsoft Azure IoT Solution Accelerator. This accelerator will provision the components required to implement the IoT Remote Monitoring solution. This includes an Azure Resource Group, IoT Hub, Azure Storage account, App Service for hosting the monitoring web application, Device Provisioning Services, Virtual Machine, and related components that host the Function Application microservices responsible for creating, capturing, and processing IoT device messages, and a Cosmos DB instance for storing reference, alert, and telemetry data.

### Help references

|                                                      |                                                                                |
| ---------------------------------------------------- | :----------------------------------------------------------------------------: |
| **Description**                                      |                                   **Links**                                    |
| Remote Monitoring solution accelerator for Azure IoT | <https://www.azureiotsolutions.com/Accelerators#description/remote-monitoring> |

### Task 1: Provision the Remote Monitoring Solution

1. Open a new web browser tab, and access **<https://www.azureiotsolutions.com>**.

2. Select the **User Icon** in the upper right corner, and sign in with your **Azure Credentials**.

    ![Sign in menu item is displayed below selected user image.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image175.png 'Sign in')

3. In the **Deploy a Microsoft solution accelerator** section, select the **Remote Monitoring** solution.

    ![A list of available Microsoft solution accelerators is displayed, the Remote Monitoring Solution is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image176.png 'Deploy the Remote Monitoring Solution')

4. This will bring you to a description screen for the Remote Monitoring Accelerator. This screen provides details surrounding the benefits of the template, the documentation, GitHub links, and the Azure Services that will be provisioned. Provisioning of these Azure Services is automated. Press the **Try Now** button to start the provisioning process.

5. The first step in provisioning the accelerator is to provide a deployment name. Enter **iot-remote-monitoring**, select the desired Azure subscription, for Deployment Options, select **C# Microservices**, then select the Azure Location nearest you. Finally, press the **Create** button.

    ![The remote monitoring creation form is displayed.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image177.png 'Remote Monitoring Solution Provisioning')

6. The second step to provisioning the accelerator requires no intervention on your part. A checklist of provisioning steps will be displayed and is updated in real-time as they are completed. Please wait until provisioning has completed.

    ![A checklist of automated deployment steps is displayed.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image178.png 'Provisioning Checklist')

7. Once completed, you will be shown a success screen with a link allowing you to view your installed solution accelerator.

    ![The remote monitoring solution has completed, a link is displayed that will open the deployed remote monitoring solution.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image179.png 'Provisioning Completed')

8. Alternatively, you may access installed accelerators in your account by selecting the **My Solutions** link on the [Azure IoT Solution Accelerators website](https://www.azureiotsolutions.com). Selecting an accelerator from this list will bring you to the screen shown in the previous step, where you can follow the **Go to your solution accelerator** link.

    ![The My Solutions button is selected and a list of deployed solution accelerators for the current logged in user is displayed.The IoT Remote Monitoring Solution is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image180.png 'Listing of deployed Azure IoT Accelerators.')

9. Upon launching the Remote Monitoring accelerator, you may be prompted to consent to accessing the Azure Portal to view the resources provisioned. Simply check the **Consent on behalf of your organization** and press the **Accept** button.

    ![A Consent Dialog is displayed, and the check box for Consent on behalf of your organization is checked. The Accept button is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image181.png 'Consent Dialog')

10. When the Remote Monitoring solution is loaded, it will bring you into the Dashboard view. A series of simulated devices are feeding the portal with live data that is displayed in maps, charts, and metrics.

    ![The Remote Monitoring Dashboard is displayed showing a left hand menu and a series of charts.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image183.png 'Remote Monitoring Dashboard')

11. Make a note of the URL of your Remote Monitoring web application - you will be utilizing this throughout this lab.

12. In the Azure Portal, you are able to see a listing of the resources provisioned to support the Remote Monitoring Accelerator solution. Access the [Azure Portal](https://portal.azure.com) and log in with your Azure credentials.

13. View the resources provisioned by the accelerator by choosing **Resource Groups** in the left-hand menu, then selecting the resource group that you entered in step 5 of this exercise, **iot-remote-monitoring**.

    ![A list of Azure Resources created with the Remote Monitoring Solution Accelerator is displayed.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image182.png 'Remote Monitoring Solution Accelerator Resources')

### Task 2: Stop running device simulation in the Remote Monitoring Solution

Navigate to the Remote Monitoring web application using the URL copied in the previous task. Stop the current device simulation, since you will be creating your own.

*Tasks to complete:*

- Navigate to the Remote Monitoring web application and stop the current device simulation.

*Exit criteria:*

- The device simulation is stopped.

- You have navigated through the site to take note of its features.

    You will notice an interactive map as the centerpiece, provided by the Azure Maps service that was provisioned as part of the solution. This will display each of your IoT devices that have location information.

    - To the left of the map is a count of the devices, as well as the number of alarms and warnings that have been triggered based on pre-configured rules. We will add custom rules later on.

    - Directly to the right of the map is a list of system alarms for the displayed devices.

    - Beneath the map is a flowing line chart that displays telemetry data for selected devices and data points.

    - To the right of the chart is a list of system KPIs (key performance indicators) that shows the number of alarms by device type and whether that number is increasing or decreasing.

    ![The Remote Monitoring Dashboard is displayed, the key performance indicators chart in the lower-right corner.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image183.png 'Remote Monitoring Dashboard')

## Exercise 2: Provision additional Azure services

**Duration:** 45 minutes

Use the Azure portal to create services that will be used by the overall IoT solution environment. You will create an additional consumer group on the default messages/events IoT Hub endpoint. This will allow us to add more readers that can read the massive amounts of incoming IoT data being ingested by IoT Hub. This will help prevent conflicts from multiple readers attempting to modify the checkpoints and offsets of the partitions holding the messages. Next, you will review the already provisioned Azure Time Series Insights instance to collect, filter, and display all-time series data flowing in from your simulated IoT devices as well as your IoT Edge device. In this exercise, you will provision a service bus queue to house engine alerts, a container registry to hold the custom Vehicle Telemetry module, and additional storage account containers that are required later in the lab.

### Help references

|                                                         |                                                                                           |
| ------------------------------------------------------- | :---------------------------------------------------------------------------------------: |
| **Description**                                         |                                         **Links**                                         |
| Remote Monitoring preconfigured solution with Azure IoT |              https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet              |
| What is Azure Time Series Insights?                     | https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-overview |

### Task 1: Create Service Bus queue

In this task, you will provision a new Service Bus queue that will be used for routing special bus engine-related critical alerts. Afterward, you will create an Azure Function triggered by incoming IoT Hub messages and route filtered messages to this queue.

1. Using a new tab or instance of your browser navigate to the **Azure Management** portal, <http://portal.azure.com>.

2. Expand the left menu and select **+ Create a Resource**, then type **Service Bus** into the search box. Select **Service Bus** from the results.

    ![In the Azure Portal, in the New blade, the search field is set to Service Bus.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image20.png 'Create a service bus resource')

3. Select the **Create** button on the Service Bus overview blade.

4. On the Create namespace screen, specify the following configuration options:

    a. Select the **Subscription** where the accelerator was deployed.

    b. Select a **Resource Group**, ensuring it's the same one in which the accelerator components have been created.

    c. **Namespace Name**: Unique value for the namespace name (ensure the green check mark appears).

    d. Select the same **Location** as your Resource Group and other services.

    e. Select the **Standard** pricing tier.

    ![The Create Namespace blade displays with the previously defined settings.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image21.png 'Create Namespace blade')

5. Select **Create**.

6. Navigate to the new **resource** after it has been created.

7. On the overview blade of your Service Bus, select **+ Queue** to create a new queue.

    ![In the top menu of the namespace blade, the + Queue button is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image22.png 'Add a queue to the service bus')

8. On the **Create queue** blade, specify the following configuration options:

    a. **Name**: alert-q

    b. **Max queue size**: 1 GB

    c. Leave the remaining values at their defaults. It is important to note that you must **not** enable duplicate detection or sessions on queues These settings will cause unexpected results and failures if applied.

    ![The Create queue blade displays with the previously defined settings.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image23.png 'Create queue blade')

9. Select **Create**.

### Task 2: Create the Critical Alerts container in Cosmos Db

1. Open your Azure Cosmos DB account by opening your resource group, and then selecting the Azure Cosmos DB account name. Select **Data Explorer** from the left-hand menu.

    ![Data Explorer is selected in the Azure Cosmos DB account blade.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image124.png 'Azure Cosmos DB account blade')

2. There are currently two databases and two containers:

    a. **Database**: pcs-storage

    - **Container**: pcs-storage

      - **Description**: contains configuration data for the IoT Remote Monitoring solution.

    b. **Database**: pcs-iothub-stream

    - **Container**: alarms

      - **Description**: This is the container that holds all IoTHub messages that the remote monitoring solution receives.

3. Select **New Container**.

    ![In the Azure Cosmos DB account blade, the New Container button is selected. ](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image125.png 'Azure Cosmos DB new container blade')

4. In the Add Collection form, specify the following:

    a. Select **Use existing** radio button.

    b. **Database Id**: pcs-iothub-stream

    c. **Container Id**: critical-alerts

    d. **Partition key**: /vin

    e. **Throughput**: 1000

    ![Fields in the Add Container blade display with the previously defined settings.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image126.png 'Add Container blade')

5. Select **OK**.

6. Next, we will obtain the connection string for Cosmos DB. From the left-hand menu, in the **Settings** section, select **Keys**. Then copy the **Primary Connection String** value and save it for use later on in this lab.

   ![In the keys section of Cosmos DB blade is selected, the copy button after the Primary Connection String value is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image239.png 'Obtain the Cosmos DB primary connection string')

### Task 3: Review the consumer groups in the IoT Hub

In this task, you will review the consumer groups that were added to the default messages/events IoT Hub endpoint, for other Azure resources to use. A consumer group can have a single reader and keeps track of items that have already been read, and what still remains to be read. Define a consumer group for each subscriber of IoT Hub event data.

1. Using a new tab or instance of your browser navigate to the **Azure Management** portal, <http://portal.azure.com>.

2. Browse to the **Resource Group** that was automatically created in the previous exercise. You can find your resource groups by selecting Resource groups in the left-hand side menu on the portal.

3. Locate your **IoT Hub** within the resource group. Its name will start with "iothub-", followed by randomly generated characters.

    ![In Resource group list, the IoT Hub is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image24.png 'Resource group listing')

4. In the left-hand menu, select the **Built-in endpoints** item, this will bring up the **Events** endpoint information and consumer groups.

    ![The built-in endpoints menu item is selected, beneath the Events section, consumer groups are listed.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image221.png 'IoT Consumer Groups')

5. Create a new consumer group by typing **queuefuncconsumergroup** into a Consumer Groups text box, we will use this consumer group later on in this lab.

![The list of consumer groups is displayed, in the textbox at the end of the list, queuefuncconsumergroup is typed in.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image222.png 'IoT Hub Create Consumer Group')

   Next, we will obtain a connection string for this IoT Hub for use later on in this lab. Select **Shared access policies**, then select **iothubowner**, and copy the primary key connection string.

   ![In the IoT Hub menu, Shared Access policies is selected. In the list of policies, iothubowner is selected, in the policy blade the copy button next to the Connection string - primary key text box is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image240.png)

### Task 4: Review the Azure Time Series Insights instance

Azure Time Series Insights is the first fully managed time series database on the Azure platform. It was developed primarily with high volume IoT data in mind, where having a single location in which you can quickly view this information and derive insights on it is typically no small feat. Although the IoT Remote Monitoring solution you provisioned stores its simulated device data in Cosmos DB, you will be able to ingest that same data into Time Series Insights, along with data generated by your IoT Edge device. This is because all data flows through IoT Hub as the initial point of ingress. You have a time series consumer group on the Events endpoint specifically for Time Series Insights to be able to simultaneously read and store the same data that will land in Cosmos DB, as well as the added IoT Edge data. After reviewing the Time Series Insights instance, you will see how it's configured to use the IoT Hub consumer group as an input.

1. Using a new tab or instance of your browser navigate to the **Azure Management** portal, <http://portal.azure.com>.

2. Select the resource group that you created when deploying the Remote Monitoring Accelerator. In the list of resources, select the item with the type **Time Series Insights environment**.

    ![In the list of resources, the Time Series Insights environment is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image184.png 'Time Series Insights environment')

3. Select **Event Sources** from the left-hand menu.

4. View the IoT Hub Event Source.

    ![Event Sources is selected under Environment Toplogy, and the Add button is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image185.png 'Event Sources blade')

### Task 5: Provision an Azure Container Registry

IoT Edge devices use one or more modules to perform a series of actions locally on the device before sending data up to the cloud. Modules include custom modules written in a language like C\#, Azure Stream Analytics, Azure Machine Learning, Azure Functions, Azure Storage, and SQL Azure. Each of these modules is hosted within a Docker container. We will be adding three modules for the IoT Edge device: a custom C\# module, an Azure Stream Analytics module, and the Storage Module. Modules are defined with all required files in a container image. The images are then pushed to a registry that stores and manages them. The final step is to deploy the images from the registry onto your IoT Edge devices. Two popular Docker registry services available in the cloud are Azure Container Registry and Docker Hub. We will be using Azure Container Registry to manage and deploy the IoT Edge modules.

1. Using a new tab or instance of your browser navigate to the **Azure Management** portal, <http://portal.azure.com>.

2. Select **+ Create a resource**, then type **container registry** into the search box on top. Select **Container Registry** from the results.

    ![The search field in the New blade displays Azure Container Registry.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image36.png 'New blade')

3. Select the **Create** button on the **Container Registry overview** blade.

4. On the **Create container registry** form, specify the following configuration options:

    a. Select the **Subscription** you used when deploying the accelerator.

    b. Select your **Resource Group**, ensuring it's the same one in which your new components have been created.

    c. **Registry Name**: Unique value for the registry name (ensure the green check mark appears).

    d. Select the same **Location** as your Resource Group and other services.

    e. Select the **Basic** SKU.

    ![The Create container registry blade displays with the previously defined settings.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image37.png 'Create container registry blade')

5. Select **Create**.

6. After provisioning is complete, go to your new Container Registry resource and select **Access keys** from the left-hand menu.

7. Enable the **Admin user**.

8. Copy the **Login server**, **Username**, and **Password** values and save them for later.

    ![In the Container registry blade, under Settings, Access keys is selected. The copy buttons for Login server, Username, and password are all called out.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image38.png 'Container registry blade')

### Task 6: Obtain the Storage Account Connection String

1. Return to the Resource Group that houses the resources for the IoT Remote Monitoring solution. From the list, select the Storage account resource whose name shares the same last 5 characters as the IoT Hub name.

   ![In the list of resources, the storage account that shares the same suffix as the IoT Hub is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image241.png 'Select the Storage Account Resource')

2. From the left-hand menu, select **Access keys**, then copy the value of the Connection string of **key1**. We will be using this value later on in the lab.

    ![In the Storage Account screen, access keys is selected from the left menu. The copy button is selected next to the key1 Connection string textbox.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image242.png 'Copy the connection string for key 1')

3. Also save the name of the storage account, as we'll be needing this value later on in this lab as well.

### Task 7: Create storage account containers required for the lab

While we are in the storage account, we will need to create two blob containers for use in this lab.

*Tasks to complete:*

- Create two blob containers, both with the `Container` public access level.

*Exit criteria:*

- You have a container named `asa-container`, which is responsible for housing the definition and configuration of the Azure Stream Analytics job that we will have running on an Edge device later in this lab.

- You have a container named `telemetrysink`, which is responsible for holding the 'cold' telemetry data coming in from an Edge device. This data is a history of all telemetry gathered from an Edge device that is not uploaded to the cloud in real time.

### Task 8: Retrieve Secrets from the Key Vault

Later in this lab, we will be making use of the Azure Key Vault. This vault is created so that secrets do not need to reside in code. A Key Vault resource was generated when we created the IoT Remote Monitoring accelerator.

*Tasks to complete:*

- Return to the resource group that was created for this lab. In the list of resources select the Azure Key Vault resource. Record the name of the Key Vault for use later on in this lab.

- Copy the current version of the `aadAppId` secret.

- Copy the current version of the `aadAppSecret` secret.

*Exit criteria:*

- You have saved both secrets to Notepad or similar text editor, to be used later in the team challenge.

## Exercise 3: Create bus and traffic light simulated devices, and add alerts and filters

**Duration:** 60 minutes

The IoT Remote Monitoring solution allows you to provision and collect telemetry from both simulated and real devices. As part of the process, telemetry schema information is applied to the devices twin through its reported properties. The properties are read as the microservice that's executing the EventProcessor processes incoming messages from IoT Hub. The EventProcessor is an Azure Function that was deployed as part of the Remote Monitoring Accelerator. The telemetry metadata is written to Cosmos DB, and used by the Remote Monitoring web UI to extract and display the data in the charts and maps. The metadata is also used to define cloud-to-device messages and actions that can be performed on the device. The web UI uses this information to send control messages to the devices. This metadata is added to simulated and non-simulated devices alike.

In this exercise, you will define metadata for new device types that will be provisioned, and whose telemetry will be simulated by the solution. Each new device type will have a state script to generate telemetry that changes the device's state, such as speed, location, voltage, or other data that relates to the device. In addition, you will define cloud-to-device messages and actions for the new device types. Then, you will create and run a new simulation locally, using a Visual Studio solution. Finally, you will create new alerts and filters through the Remote Monitoring web app interface.

We have created the following files for you, located within the device-simulation project (Lab-files/DeviceSimulation/Services/data/devicemodels):

- Device models:

  - bus-01.json

  - bus-02.json

  - trafficlight-01.json

  - trafficlight-02.json

- Scripts (/scripts subfolder):

  - bus-01-state.js

  - bus-02-state.js

  - trafficlight-01-state.js

  - trafficlight-02-state.js

  - DecreaseTiming-method.js

  - IncreaseTiming-method.js

You will need to finish configuring these files for the simulator.

### Help references

|                                             |                                                                                                   |
| ------------------------------------------- | :-----------------------------------------------------------------------------------------------: |
| **Description**                             |                                             **Links**                                             |
| IoT Remote Monitoring device models specs   |              <https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models>               |
| Creating and managing simulations           |   <https://github.com/Azure/device-simulation-dotnet/wiki/%5BAPI-Specifications%5D-Simulations>   |
| Use rules to detect issues                  |      <https://docs.microsoft.com/en-us/azure/iot-suite/iot-suite-remote-monitoring-automate>      |
| Customize the device simulator microservice |        <https://docs.microsoft.com/en-us/azure/iot-suite/iot-suite-remote-monitoring-test>        |
| IoT Remote Monitoring solution architecture | <https://docs.microsoft.com/en-us/azure/iot-suite/iot-suite-remote-monitoring-sample-walkthrough> |

### Task 1: Configure the Device Simulation projects to run locally

In this task, you will open the device-simulation solution in Visual Studio 2019 and configure the projects to run locally.

1. Browse to the device-simulation solution in the following location: Lab-files\\DeviceSimulation.

2. Open **device-simulation.sln**.

    ![File Explorer displays with the previous path and the device-simulation.sln file called out.](media/device-simulation-folder-explorer.png 'File Explorer')

3. Right-click on the **WebService** project, and select the **Set as StartUp Project** option.

   ![The Web Service context menu is displayed with the Set as StartUp Project option selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image264.png)

4. In the Visual Studio toolbar, ensure the startup item is the WebService project.

    ![The Visual Studio Taskbar is displayed, the start up item drop down list is expanded with the WebService item selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image265.png)

5. Right-click on the **WebService** project and select **Properties** from the left-hand menu. Select **Debug** from the left side of the Properties window, and in the Environment variables section, populate the values for the following Environment Variables (the others can remain empty or keep their default values):

    | Environment Variable                      | Value                                 |
    |-------------------------------------------|---------------------------------------|
    | PCS_STORAGEADAPTER_WEBSERVICE_URL         | http://localhost:9022/v1              |
    | PCS_IOTHUB_CONNSTRING                     | *your IoT Hub Connection String*      |
    | PCS_AZURE_STORAGE_ACCOUNT                 | *your Azure Storage Connection String*|
    | PCS_STORAGEADAPTER_DOCUMENTDB_CONNSTRING  | *your CosmosDB Connection String*     |

    ![On the WebService project properties, Debug tab is selected, and in the Environment variables section, the previous values are populated in the form.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image187.png 'WebService project debug properties')

6. Save your changes to the file.

### Task 2: Finish configuring the simulated IoT device models and scripts

In this task, you will finish configuring the device models we have provided for you.

1. With the device-simulation solution still open, use Solution Explorer to expand the **Services** project. Next, open **bus-01.json** located under **data\\devicemodels**.

    ![The following path is expanded in Solution Explorer: Services\Data\devicemodels. Under devicemodels, bus-o1.json is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image44.png 'Solution Explorer')

2. Set the following Simulation.InitialState values. These are the parameters used at the start of the device simulation:

    a. **latitude**: 40.755086

    b. **longitude**: -73.984165

    c. **fuellevel**: 70.0

    d. **speed**: 30.0

    e. **vin**: Y3J9PV9TN36A4DUB9

    ![The initial state values are highlighted in the JSON code window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image45.png 'JSON code window')

3. Set the following Properties:

    a. **Type**: Bus

    b. **Location**: Contoso

    c. **Latitude**: 40.755086

    d. **Longitude**: -73.984165

    ![The properties values are highlighted in the JSON code window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image46.png)

4. There are two Telemetry schemas set for this bus. The first one should send telemetry every 10 seconds, while the other one should have an interval of one minute. Complete the Telemetry values according to the following specifications:

    a. Telemetry \#1:

    - **MessageSchema.Fields**:

      - **latitude**: double

      - **longitude**: double

      - **speed**: double

      - **speed_unit**: text

      - **vin**: text

    b. Telemetry \#2:

    - MessageTemplate (use the MessageTemplate value of the first telemetry as a guide): Include the fuellevel and fuellevel_unit fields.

    - **MessageSchema.Fields**:

      - **fuellevel**: double

      - **fuellevel_unit**: text

    ![The previously defined telemetry values are circled in the JSON code window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image47.png 'JSON code window')

5. Save your changes.

6. Open **bus-02.json**.

7. Set the following Simulation.InitialState values. These are the parameters used at the start of the device simulation:

    a. **latitude**: 40.693935

    b. **longitude**: -73.952279

    c. **fuellevel**: 53.0

    d. **speed**: 42.0

    e. **vin**: 2K0H7PNZY0RSFQ033

    ![The initial state values are highlighted in the JSON code window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image48.png 'JSON Code Window')

8. Set the following Properties:

    a. **Type**: Bus

    b. **Location**: Tailwind

    c. **Latitude**: 40.693935

    d. **Longitude**: -73.952279

    ![The properties values are highlighted in the JSON Code window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image49.png 'JSON Code window')

9. There are two Telemetry schemas set for this bus. The first one should send telemetry every 12 seconds, while the other one should have an interval of 55 seconds. Complete the Telemetry values according to the following specifications:

   - Telemetry \#1:

   - MessageSchema.Fields:

       - **latitude**: double

       - **longitude**: double

       - **speed**: double

       - **speed_unit**: text

       - **vin**: text

   - Telemetry \#2:

   - MessageTemplate (use the MessageTemplate value of the first telemetry as a guide): Include the fuellevel and fuellevel_unit fields.

   - MessageSchema.Fields:

     - **fuellevel**: double

     - **fuellevel_unit**: text

     ![The previously defined telemetry values are highlighted in the JSON code window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image50.png 'JSON code window')

10. Save your changes.

11. Open **trafficlight-01.json**.

12. Add the following **CloudToDeviceMethods**:

    - IncreaseTiming

      - **Type**: javascript

      - **Path**: IncreaseTiming-method.js

    - DecreaseTiming

      - **Type**: javascript

      - **Path**: DecreaseTiming-method.js

    ![The previously defined methods are highlighted in the JSON code window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image51.png 'JSON Code Window')

13. Save your changes.

14. Open **trafficlight-02.json**.

15. Add the same **CloudToDeviceMethods** that you added to trafficlight-01.json.

16. Save your changes.

17. In Solution Explorer, expand the scripts subfolder (Services\\data\\devicemodels\\scripts), then open **bus-01-state.js**.

18. Scroll down to the **main** function and complete the following lines of code:

    a. Find TODO: 1 and complete the line of code underneath to set the state.speed value to a random double value with an average of 30, \~40%, a minimum value of 0 and a maximum of 80.

    b. Find TODO: 2 and complete the line of code underneath to set the state.fuellevel value to a random double value with an average of 70, \~25%, a minimum value of 2 and a maximum of 80.

    c. Your finished code should look like the following:

    ```javascript
    // 30 +/- 40%,  Min 0, Max 80
    // TODO: 1 - finish this line of code:
    state.speed = vary(30, 40, 0, 80);

    // 70 +/- 25%,  Min 2, Max 80
    // TODO: 2 - finish this line of code:
    state.fuellevel = vary(70, 25, 2, 80);
    ```

19. Save your changes.

20. Open **bus-02-state.js**.

21. Scroll down to the **main** function and complete the following lines of code:

    a. Find TODO: 3 and complete the line of code underneath to set the state.speed value to a random double value with an average of 42, \~50%, a minimum value of 0 and a maximum of 80.

    b. Find TODO: 4 and complete the line of code underneath to set the state.fuellevel value to a random double value with an average of 53, \~25%, a minimum value of 2 and a maximum of 80.

    c. Your finished code should look like the following:

    ```javascript
    // 42 +/- 50%,  Min 0, Max 80
    // TODO: 3 - finish this line of code:
    state.speed = vary(42, 50, 0, 80);

    // 53 +/- 25%,  Min 2, Max 80
    // TODO: 4 - finish this line of code:
    state.fuellevel = vary(53, 25, 2, 80);
    ```

22. Save your changes.

23. Open trafficlight-01-state.js and observe how it is altering values over time. You will see that it is varying the light state (red, green, yellow), as well as the voltage. If you look at trafficlight-02-state.js, you will notice that its voltage range is significantly higher than for the first traffic light. When we create an alert, this will most likely be the light that triggers it.

24. Open DecreaseTiming-method.js and see how it is used as an action method to reset the timing state of a traffic light by decreasing the timing 15 seconds at a time. This is executed by a cloud-to-device call from the monitoring web app.

### Task 3: Explore the remaining files to understand what is happening

Below is a table containing file paths and an explanation of what each does in the simulator. There are a few key things to point out so that you know how the Service SDK for Azure IoT Devices can be used to programmatically manage devices.

1. With the device-simulation solution still open in Visual Studio, look at each of the following files and descriptions to understand how things work:

    - Visual Studio Project: Services
      - **File Path**: Devices.cs
      - **Description**: GetAsync method (line 102) accepts a Device Id and uses it to retrieve the device details from IoT Hub, using the Service SDK's RegistryManager. It will optionally retrieve the device twin which can be used to view the current twin properties and update their values. CreateAsync method (line 156) is used to provision a new IoT Device, using the RegistryManager. It also creates a new device twin containing the IsSimulated tag. This is how the IoT Monitor app can differentiate between simulated and physical devices.

    - Visual Studio Project: Services
        - **File Path**: DeviceClient.cs
        - **Description**: SendMessageAsync method (line 100) constructs a new Message object that it will send to IoT Hub. It includes the event message properties to include the message content type (JSON), and the schema name, as defined in the device model scripts you edited earlier. The microservice running the IoT Hub EventProcessor will look for these values before processing messages and saving them to Cosmos DB.

    - Visual Studio Project: SimulationAgent
        - **File Path**: SimulationManager.cs
        - **Description**: Each device is assigned an instance of a DeviceStateActor (DeviceState\\DeviceStateActor.cs), DeviceConnectionActor (DeviceConnection\\DeviceConnectionActor.cs), DeviceTelemetryActor (DeviceTelemetry\\DeviceTelemetryActor.cs), DevicePropertiesActor(DeviceProperties\\DevicePropertiesActor.cs), and DeviceReplayActor(DeviceReplay\\DeviceReplayActor.cs). This class manages all of the actors for this solution - generated through the *TryToCreateActorsForPartitionAsync* and *CreateActorsForDeviceAsync*. Through the actors, the simulated device will:
            - Connect to IoT Hub.
            - Bootstrap the device to retrieve it, create if necessary, and update the device twin state.
            - Update the device state using the state scripts we created, in order to send telemetry.
            - Sends telemetry using the message template provided, as seen in the bus and traffic light device model scripts you edited earlier. Uses the DeviceClient class to send the message through the Device SDK for Azure IoT Devices.

    - Visual Studio Project: WebService
        - **File Path**: v1\\Controllers\\SimulationsController
        - **Description**: This web API controller contains REST methods that allow you to retrieve, insert, update, and delete device simulations. The simulation values are updated in Cosmos DB by way of the Simulations service (Services project -- Simulations.cs), which uses the running pcs-storage-adapter service to modify the values in Cosmos DB. We will be using this next.

### Task 4: Configure and run the Storage Adapter project

The Storage Adapter project (pcs-storage-adapter) is another microservice that constantly runs and provides REST-based endpoints to manage simple key/value data in Cosmos DB. It is used by several services, including the web service within the device-simulator project, as seen in the previous task. This needs to be configured, then executed to run before creating and running simulations on the new devices locally.

1. Browse to the storage-adapter solution in the following location: LabFiles\\StorageAdapter.

2. Open **storage-adapter.sln**.

    ![File Explorer is open to the the previously defined path, and the storage-adapter.sln file is selected.](media/storage-adapter-file-explorer.png 'File Explorer')

3. Right-click on the **WebService** project, and select the **Set as StartUp Project** option.

   ![The Web Service context menu is displayed with the Set as StartUp Project option selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image264.png)

4. In the Visual Studio toolbar, ensure the startup item is the WebService project.

    ![The Visual Studio Taskbar is displayed, the start up item drop down list is expanded with the WebService item selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image265.png)

5. Right-click the **WebService** project in the Solution Explorer, then select **Properties**.

    ![In Solution Explorer, WebService is selected, and on its right-click menu, Properties is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image53.png 'Solution Explorer')

6. Select **Debug** from the left-hand menu. In the Environment variables section, populate the following variables:

    | Environment Variable         | Value                                 |
    |------------------------------|---------------------------------------|
    | PCS_KEYVAULT_NAME            | *your key vault name*                 |
    | PCS_AAD_APPID                | *the value for aadAppId from the Key Vault*  |
    | documentDBConnectionString   | *your Cosmos DB Connection String*    |
    | PCS_AAD_APPSECRET            | *the value for aadAppSecret from the Key Vault*     |

    ![WebService Project Properties Debug Settings panel is displayed with the Environment Variables from the table above defined.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image268.png)

7. Save your changes to the file.

8. Right-click the **WebService** project once again, then select **Start new instance** under **Debug**.

    ![In Solution Explorer, WebService is selected, and from its right-click menu, Debug, and then Start new instance are selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image57.png 'Solution Explorer')

9. Starting the project will bring up a console window indicating the service is now listening on port 9022.

    ![A console window is displayed indicating the application is now listening for HTTP requests on port 9022](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image266.png)

10. Open a new browser window and go to the following path: <http://localhost:9022/v1/status>. You should also see a status response on the page showing the service is alive and well.

    ![The status response of "Alive and well" is highlighted in the Web service window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image58.png 'Web service window')

11. Leave the project running in debug mode.

### Task 5: Run the Simulator web service and create a new simulation

In this task, you will run the Simulator web service locally and send REST-based commands to it to delete the existing simulation and define a new one using only the devices we want to simulate for the lab, including the new device types.

1. Switch back to the **device-simulation** solution in Visual Studio. Right-click the **WebService** project, then select **Start new instance** under **Debug**, to run a new instance of the web app.

    ![In Solution Explorer, WebService is selected, and from its right-click menu, Debug, and then Start new instance are selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image59.png 'Solution Explorer')

2. This will launch a new command window with console statements flowing through.

   ![Console window for device simulation is displayed.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image247.png 'Device Simulation Console Window')

3. Open **Postman**. You should have it installed from the lab's prerequisites. If not, refer to the link to install Postman found there.

4. Add a new request. Select the **POST** method and enter <http://localhost:9003/v1/simulations> as the URL.

5. Select **Headers** beneath the URL and add the following Key / Value pair:

    a. **Key**: Content-Type

    b. **Value**: application/json

    ![In the Postman window, both Post and the previously defined URL are selected. Below, the Headers tab is selected, and the Content-Type key with an applicaton/json value is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image62.png 'Postman window')

6. Select **Body**, select **raw**, then **JSON (application/json)** as the content type. Paste the following to create a new simulation with our new models - remember to add your IoT Hub Connection string to the **IoTHubs** property:

    ``` javascript
    {
        "Name": "Smart City Simulation",
        "Enabled": true,
        "IoTHubs": [
            {
                "ConnectionString": "YOUR_IOT_HUB_CONNECTION_STRING"
            }
            ],
            "DeviceModels": [
                {
                    "Id": "bus-01",
                    "Count": 1
                },
                {
                    "Id": "bus-02",
                    "Count": 1
                },
                {
                    "Id": "trafficlight-01",
                    "Count": 1
                },
                {
                    "Id": "trafficlight-02",
                    "Count": 1
                }
            ]
    }

    ```

    ![In the Postman window, on the Create a new IoT Hub tab, on the Body tab, raw is selected. JSON (application/json) is selected, and the JSON code as previously defined displays below.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image63.png 'Postman window')

7. Press **Send**. You should receive a response status of 200 OK, and output showing the new simulation information with your defined devices. The Enabled value should also be true.

    ![In the Postman window, Send is selected, Status: 200 OK displays, and in the Body, "Enabled"; True, is called out.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image64.png 'Postman window')

8. Leave this project running in the background.

### Task 6: Create alerts and filters in the monitoring web app

The IoT Remote Monitoring web interface enables you to create filters that help group devices by type or other parameters. You can also create alerts that are fired when certain criteria are met, enabling you to see the alerts alongside your device data or on the map. In this task, you will create filters for your buses and traffic lights, then create an alert for traffic lights whose voltage exceed a predefined level.

1. Navigate back to the monitoring web app. If you don't remember the path or have closed the previous browser session, the naming convention is **https://[your solution name].azurewebsites.net/dashboard**. You may need to refresh the browser window if it has been running for some time and is unresponsive.

2. One of the first things you may notice is that there are new telemetry data points listed above the graph. You should also see new devices showing up on the map. In the screenshot below, the new fuel level telemetry option is selected, and data for the two new buses appear beneath. (Note: You may need to wait a few minutes with the simulator running in order for the New York data points to appear - zoom out the map to see them.)

    ![A map displaying the location of devices with a map marker.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image67.png 'Device Maps')

    ![A chart displaying the fuel level consumption of simulated bus devices.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image191.png 'Fuel level telemetry chart')

3. Create a new device group by selecting **Manage device groups** on the upper-right portion of the dashboard.

    ![The manage device groups button is selected from the top menu of the dashboard.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image68.png 'Manage device groups')

4. Select **+ Create new device group**, then provide the following parameters in the form:

    a. **Name**: Buses

    b. **Field**: Properties.Reported.Type

    c. **Operator**: = Equals

    d. **Value**: Bus

    e. **Type**: Text

    ![In the manage device groups settings fields are set to the previously defined settings.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image69.png 'Create device group section')

5. Select **Save**.

6. Create another device group with the following parameters in the form:

    a. **Name**: Traffic Lights

    b. **Field**: Properties.Reported.Type

    c. **Operato**r: = Equals

    d. **Value**: Traffic Light

    e. **Type**: Text

7. After creating both device groups, you may select them using the filter drop-down list on the top of the **Dashboard** screen. In the screenshot below, we have selected Buses. Notice that the shows only the two Bus devices.

    ![The Monitoring Web App dashboard displays with filtered by the selected group of devices.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image70.png 'Monitoring Web App dashboard filtered by device group')

8. Select **Rules** in the left-hand menu.

9. Change the filter to show All devices so you can view the list of existing rules. Each one has a unique name and description, are marked with a severity level, and have filters and triggers to apply the rule to specific devices and act on certain criteria. Select **+ New rule** to create a new rule for the traffic lights.

    ![The Rules menu item is selected, the device group filter is set to All devices, and the + New rule button is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image71.png 'Rules section')

10. Specify the following values in the New rule form:

    a. **Rule Name**: Voltage Too High

    b. **Description**: Traffic light voltage is higher than normal.

    c. **Device group**: Select your Traffic Lights group.

    d. **Calculation**: Select Instant.

    e. **Condition1**:

    - **Field**: voltage

    - **Operator**: \>=

    - **Value**: 74

    f. **Severity level**: Critical

    g. **Rule status**: Enabled

    ![The New rule form is displays and field values are set to the previously defined settings.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image72.png 'New rule form')

    ![A continuation of the new rule form with fields set to the previously defined settings.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image192.png 'New rule section')

11. Select **Apply**. Notice that it shows 2 devices are affected by this rule.

12. Navigate back to the dashboard. It may take a few minutes for the alerts to start appearing. When you filter by Traffic Lights and zoom in on the map over New York, you will see both traffic lights pinned to the map. One with the critical alert. Also notice the alarm count on the left.

    ![The remote monitoring solution dashboard is set to filter on the Traffic Lights device group. The Alarm count is 1, and a voltage alert is called out in the Alerts list.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image73.png 'Monitoring Web App dashboard ')

13. Select the **traffic light** with the error indicator on the map. The device details will be displayed to the right, along with a list of the triggered alarms.

14. Select **Maintenance** on the left-hand menu.

15. Select the **Voltage Too High** alert. You will see a list of occurrences and devices that triggered the alarm. Using the menu on top, you can close or acknowledge alerts for each selected occurrence in the list.

    ![In the Maintenance section, two alerts for Voltage too high are selected. At the top, right, the Close and Acknowledge buttons are called out.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image74.png 'Maintenance section')

### Task 7: Send jobs to IoT devices

In this task, you will send a job to one of the traffic light devices, using the DecreaseTiming job defined in the scripts folder of the device-simulation project.

1. Navigate back to the monitoring web app's dashboard.

2. Select the **timing** telemetry option. Observe the current timing for the traffic lights. One should consistently be 90 (seconds), and the other 65.

    ![In the Telemetry section, timing (2) is selected and a chart is displayed.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image75.png 'Telemetry section')

3. Navigate to **Device Explorer** using the left-hand menu.

4. Check the box next to **GUIDVALUE.Trafficlight-01.1** (or whichever the traffic light \#1 is named in your list - the names could be long, so selecting one will open an overview blade displaying the full name of the device).

5. Select **Jobs** in the top menu.

6. Select **Methods** for the job type.

7. In the **Method Name**, select **DecreaseTiming**.

8. Provide any name for your Job.

9. Select **Apply**. You may view the job status in the maintenance page, if desired.

    ![The Device Explorer menu item is selected. The Device list is displayed with a checkbox checked next to the first device in the list. The Jobs menu item is selected from the top menu, and a Jobs form is displayed with the previously described values. The Apply button on the form is highlighted.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image76.png 'Devices section')

10. Navigate back to the dashboard and view the **timing** telemetry once again. This time, you should notice that the traffic light timing for traffic light \#1 decreased from 90 seconds to 75. (You may need to wait a couple of minutes to see the effect in the chart)

    ![In the Telemetry section, the decrease of the timing of a traffic light is highlighted on the graph.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image77.png 'Telemetry section')

## Exercise 4: Create IoT Edge device and custom modules

**Duration:** 60 minutes

Azure IoT Edge devices can run on in locations where there is little to no internet connectivity, yet they allow you to run powerful modules locally, enabling you to apply your business logic in place. This is especially powerful when coupled with sensors that generate a lot of data, and you only want to send the most important data to the cloud.

In this scenario, IoT Edge devices will be installed on city buses. You will create a Stream Analytics module to filter the vehicle telemetry data from simulated sensors located within a custom module that you will also build and deploy. When the Stream Analytics module finds behavior consistent with aggressive driving or impending engine failure, the data will be sent to Azure via IoT Hub. The obvious benefit to this is that the massive amount of data can be analyzed locally, and only the important data is sent over the expensive cellular data connection. All data is also stored on the Edge Device itself and is uploaded automatically via Blob Storage channels in the cloud once sufficient connectivity is established.

### Help references

|                                                                       |                                                                                 |
| --------------------------------------------------------------------- | :-----------------------------------------------------------------------------: |
| **Description**                                                       |                                    **Links**                                    |
| What is Azure IoT Edge?                                               |      <https://docs.microsoft.com/en-us/azure/iot-edge/how-iot-edge-works>       |
| Understand the requirements and tools for developing IoT Edge modules |      <https://docs.microsoft.com/en-us/azure/iot-edge/module-development>       |
| Develop and deploy a C\# IoT Edge module to your simulated device     |    <https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-csharp-module>     |
| Azure Stream Analytics on IoT Edge                                    | <https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-edge> |
| Azure Blob Storage on IoT Edge | <https://docs.microsoft.com/en-us/azure/iot-edge/how-to-store-data-blob> |

### Task 1: Add a new IoT Edge device

1. Navigate to the Azure Management portal, <http://portal.azure.com>.

2. Open **IoT Hub** in your solution's resource group.

3. Select **IoT Edge** from the left-hand menu, then select **+ Add to IoT Edge Device**.

    ![The Add IoT Edge Device button is selected in the IoT Hub blade.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image78.png 'IoT Hub blade')

4. On the Add Device blade, specify the following configuration options:

    a. **Device ID**: bus1

    b. Make sure the Symmetric Key **Authentication Type** is selected, and **Auto Generate Keys** is checked.

    c. **Connect device to IoT Hub** should be Enabled.

    ![In the Add Device blade, fields are set to the previously defined settings.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image79.png 'Add Device blade')

5. Select **Save**.

6. Select your new IoT Edge device from the list of devices.

7. Copy the value for **Connection string--primary key** and save it. This will be used to configure the IoT Edge runtime.

    ![In the Device Details blade, the copy button for the Connection string - primary key is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image80.png 'Device Details blade')

8. Open the device twin properties by pressing the **Device twin** button.

    ![In the Device Details blade, the the Device twin button is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image213.png 'Device Details blade')

9. Set the device twin desired properties to the following, then press **Save**:

    ```javascript
      "Protocol": "MQTT",
      "SupportedMethods": "",
      "Telemetry": {
        "bus-edge;v1": {
          "MessageSchema": {
            "Name": "bus-edge;v1",
            "Format": "JSON",
            "Fields": {
              "abs": "Integer",
              "accelerator_pedal_position": "Integer",
              "agressiveDriving": "Integer",
              "averageEngineTemperature": "Double",
              "averageSpeed": "Double",
              "brake_pedal_status": "Integer",
              "engineoil": "Integer",
              "enginetempanomaly": "Integer",
              "engineTemperature": "Integer",
              "fuel": "Integer",
              "headlamp_status": "Integer",
              "ignitionStatus": "Integer",
              "iothubConnectionModuleId": "Text",
              "odometer": "Integer",
              "oilanomaly": "Integer",
              "outsideTemperature": "Temperature",
              "parking_brake_status": "Integer",
              "speed": "Integer",
              "timestamp": "Text",
              "tirepressure": "Integer",
              "transmission_gear_position": "Text",
              "vin": "Text",
              "windshield_wiper_status": "Integer",
              "mlDetectedAggressiveDriving": "Integer"
            }
          }
        }
      },
      "Type": "Bus",
      "VIN": "Y3J9PV9TN36A4DUB9",
      "Borough": "Northwind",
      "Latitude": 40.70884,
      "Longitude": -74.01456,
    ```

    ![In the Device Twin blade, the desired properties value is set to the code snippet above and the save button is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image214.png 'Device Twin Blade')

### Task 2: Provision new Linux virtual machine to run as the IoT Edge device

In this task, you will provision a new Linux virtual machine that will be used to run the IoT Edge device, using an Azure IoT Edge on Ubuntu virtual machine available from the Azure Marketplace.

1. Open the following URL in a new browser window: <https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=overview>.

2. Log in with the same Azure account you are currently using.

3. Press the **GET IT NOW** button, then **Continue** to open the deployment template in the Azure Portal.

    ![In the Marketplace web page for Azure IoT Edge on Ubuntu, press the GET IT NOW button.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image200.png 'Azure Marketplace')

4. In the Azure portal press the **Create** button to initiate the provisioning.

5. Complete the quickstart template form with the following parameters:

    a. **Subscription**: Select the same subscription you've been using for the lab.

    b. **Resource group**: Select the same resource group you've been using.

    c. **Virtual Machine Name**: IoTEdgeVM

    c. **Region**: Should be the same as the location of your resource group and other services.

    d. **Image**: Select **Ubuntu Server 16.04 LTS + Azure IoT Edge runtime**

    e. **Size**: **Standard B1ms** is sufficient for this exercise.

    ![The Ubuntu Virtual Machine Settings form is displayed populated with the previous values.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image201.png 'Create a Virtual Machine Blade')

    f. **Authentication**: Select **Password**.

    g. **Admin Username**: **Make note of the username you enter** so you can use it later.

    h. **Admin Password**: **Make note of the password you enter** so you can use it later.

    i. **Public inbound ports**: Select **Allow selected ports**.

    j. **Select inbound ports**: Select **SSH (22)**.

    ![A continuation of Ubuntu Virtual Machine Settings form populated with the previous values.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image202.png 'Create a Virtual Machine Blade')

6. Then select **Review + create**, then after validation passes, press the **Create** button to deploy the Virtual Machine.

7. Once created, access the VMs overview blade, select **Connect**. Copy the SSH command.

    ![The ssh command is highlighted, and the Connect button is selected in the Virtual machine blade.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image82.png 'Virtual machine blade')

8. Open your Bash client and paste the SSH command, then press **Enter**.

    > **Note**: If your SSH command includes a `-i <private key path>` flag, you may optionally remove it from the command when connecting to the virtual machine.

9. When asked whether you want to continue connecting, enter **yes**.

10. Enter the password you provided when provisioned the IoT Remote Monitoring solution.

    ![In the Bash window, the SSH command and the Yes response are both called out.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image83.png 'Bash window')

11. Enter the following command to update the container device connection string (replace value with the **IoT Hub connection string** you copied in Task 1 above):

    ```bash
    sudo /etc/iotedge/configedge.sh "{IoT Hub -> IoT Edge device connection string}"
    ```

12. Execute the following Docker command to see that the IoT Edge agent is running as a module:

    ```bash
    sudo iotedge list
    ```

    ![The Bash window displays with the previous docker command.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image85.png 'Bash window')

    > If you do not see this module right away, re-run the `sudo iotedge list` command after a few seconds.

13. Next, we'll navigate to the root directory. Execute the following commands:

    ```bash
    cd ..
    cd ..
    ```

14. Next, we will create a folder to house the data for our local storage. 

    ```bash
    sudo mkdir storage
    cd storage
    sudo mkdir containerdata
    cd ..
    ```

15. Grant the default module user privileges to the directory by executing the following commands:

    ```bash
    sudo chown -R 11000:11000 /storage/containerdata
    sudo chmod -R 700 /storage/containerdata
    ```

### Task 3: Create and upload the custom C\# IoT Edge module for vehicle telemetry

In this task, you will use Visual Studio Code to complete the custom C\# IoT Edge module that simulates vehicle telemetry representing bus sensor data. Then you will create the Docker container, and register it in your Container Registry instance so it can be deployed to the IoT Edge device.

This simulator accomplishes many things. As part of the project, Fabrikam city wanted to apply machine learning to determine if a bus driver is driving dangerously. A machine learning model has been supplied in this project and is used to determine the safety of the driver by evaluating the current bus speed, with the location (latitude and longitude) of the bus. The prediction from this model is populated in the telemetry data so that it can be used for future processing.

Additionally, all telemetry obtained from the bus sensors is saved in local blob storage. The storage module is responsible for automatically uploading this data to the cloud when a feasible internet connection is established.

1. Open Visual Studio Code.

2. Select **File Open Folder**...

    ![File/Open Folder is selected in the Visual Studio Code window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image86.png 'Visual Studio Code window')

3. Browse to the lab-files in the Hands-on lab folder. Select the **VehicleTelemetrySimulator** folder.

4. You may see one or more errors about unresolved dependencies or needing to add build and debug assets. Dismiss these messages, as they are not pertinent to the IoT Edge module project.

    ![In the Visual Studio Code window, pop up messages are displayed, the Don't ask again and Close buttons are called out.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image87.png 'Visual Studio Code window')

5. Open **Program.cs** under the **modules** folder.

    ![In the Explorer window, the modules folder is open and Program.cs is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/imageModuleProgramcs.png 'Explorer window')

6. Complete the code for TODO items 1-18.

    a. The first item to complete is to add the device connection string.

    ```C#
     //TODO: 1 - set device connection string for the device client
    //static string _deviceConnectionString = "<device connection string goes here>";
    ```

    b. Set the connection string to the local IoT Edge Blob storage.

    ```C#
    //TODO: 2 - set the connection string for local blob storage
    //static string _storageConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=http://azureblobstorageoniotedge:11002/edgestorage;AccountName=edgestorage;AccountKey=pM8cWFj0L8h+VKRfE8Fy3tVVtdfOR4bCIzX8N/sDiK1X0znhu8iatFwVfjzwjedDKe5ln+2cI7wpy+2eO1vvQQ==";
    ```

    c. Create the module client by un-commenting the following line of code. The Module Client is initialized using the device connection string that we initialized our Edge VM with.

    ```C#
    // TODO: 3 - Create module client from container environment variable
    // _vehicleTelemetryModuleClient = await ModuleClient.CreateFromEnvironmentAsync(TransportType.Mqtt);
    ```

   d. Create the device client by un-commenting the following two lines of code. The device client is responsible for retrieving the desired properties from the device twin in the cloud, as well as sending reported properties to the device twin. The values that will be reported are the Latitude, Longitude, and Borough values. The second line of code hooks up a call back method that is called on the device once a desired property has been changed in the Device Twin in the cloud.

   ```C#
    // TODO: 4 - Create device client to obtain desired properties from Twin and update reported properties
    // _deviceClient = DeviceClient.CreateFromConnectionString(_deviceConnectionString);
    // await _deviceClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdateAsync, null);
   ```

   e. Use the device client to retrieve the desired properties from the Device Twin in the cloud. Use these values to populate the initial state of the current device.

   ```C#
   // TODO: 5 - initialize device instance with values obtained from the device twin desired properties
    // var twin = await _deviceClient.GetTwinAsync();
    // var desired = twin.Properties.Desired;
    // await UpdateDeviceInstanceFromDesiredProperties(desired);
   ```

    f. Initialize local edge blob storage.

    ```C#
    // TODO: 6 - initialize iot edge storage
        // _storageAccount = CloudStorageAccount.Parse(_storageConnectionString);
        // _blobClient = _storageAccount.CreateCloudBlobClient();
        // _blobContainer = _blobClient.GetContainerReference("telemetry");
        // if(!_blobContainer.Exists()){
        //     _blobContainer.CreateIfNotExists();
        // }
    ```

    g. Populate the current 'state' field values from the retrieved Desired Properties obtain from the Device Twin.

    ```C#
    if (desired["VIN"] != null)
    {
        // TODO: 7 - Set the vin to the value in the device twin
        // _vin = desired["VIN"];
    }
    if (desired["Borough"] != null)
    {
        // TODO: 8 - Set the borough to the value in the device twin
        // _borough = desired["Borough"];
    }

    if (desired["Latitude"] != null)
    {
        // TODO: 9 - Set the latitude to the value in the device twin
        // _latitude = Convert.ToSingle(desired["Latitude"]);
    }
    if (desired["Longitude"] != null)
    {
        // TODO: 10 - Set the longitude to the value in the device twin
        // _longitude = Convert.ToSingle(desired["Longitude"]);
    }
    ```

    h. Initialize a timer to send reported properties to the Device Twin at regular intervals.

    ```C#
        // TODO: 11 - update reported properties at a specified time interval
        // _timer = new Timer(UpdateReportedProperties, null, TimeSpan.FromSeconds(_secondsToTwinReportedPropertiesUpdate), TimeSpan.FromSeconds(_secondsToTwinReportedPropertiesUpdate));
    ```

    i. Implement the code that sends the current state of the device to the cloud through reported properties of the device twin.

    ```C#
        // TODO: 12 - update reported properties with the IoT Hub with most recent Lat/Long
        //patch the changed properties (Latitude, Longitude, Borough)
        // TwinCollection patch = new TwinCollection();
        // patch["Latitude"] = _latitude;
        // patch["Longitude"] = _longitude;
        // patch["Borough"] = _borough;
        // patch["Telemetry"] = _telemetryDefn;
        // Task.Run(async () => await _deviceClient.UpdateReportedPropertiesAsync(patch));
    ```

    j. Initialize the machine learning context, load its model, and create an instance of the prediction engine to evaluate the incoming telemetry for dangerous driving.

    ```C#
        // TODO: 13 - Initialize machine learning prediction model infrastructure
        // var mlContext = new MLContext();
        // ITransformer mlModel = mlContext.Model.Load("BusMlModel/MLModel.zip", out var modelInputSchema);
        // var predEngine = mlContext.Model.CreatePredictionEngine<ModelInput, ModelOutput>(mlModel);
    ```

    k. Populate the input model with the current state data of the bus. This information will be used by the prediction engine to determine if the bus driver is driving dangerously.

    ```C#
        // TODO 14: Create input for the machine learning prediction engine by setting the
        //         device current latitude, longitude, and speed limit
        // var mlInput = new ModelInput()
        // {
        //    Latitude = currRouteData.Latitude,
        //    Longitude = currRouteData.Longitude,
        //    BusSpeed = currRouteData.BusSpeed
        // };
    ```

    l. Use the prediction engine to determine if the driver is driving dangerously.

    ```C#
        // TODO 15: Use this input model to have the prediction engine determine if the
        //          current speed for the device is safe for the latitude and longitude location
        // var mlOutput = predEngine.Predict(mlInput);
    ```

    m. Populate the predicted value into the telemetry being sent by the module.

    ```C#
        // TODO: 16 Populate the machine learning prediction into the telemetry data for upstream systems
        // mlDetectedAggressiveDriving = mlOutput.Prediction
    ```

    n. Output the generated telemetry from the module asynchronously.

    ```C#
        // TODO: 17 - Have the ModuleClient send the event message asynchronously, using the specified output name
        // await _vehicleTelemetryModuleClient.SendEventAsync(outputName, message);
    ```

    o. Save all data to local blob storage

    ```C#
        // TODO: 18 - Send all telemetry to local blob storage
        // var blockBlob = _blobContainer.GetBlockBlobReference($"telemetry_{info.timestamp.Ticks}.json");
        // blockBlob.UploadText(serializedString);
    ```

7. Save your changes.

8. In VS Code, open **module.json**, in the repository property, change the URI to **LOGIN SERVER/vehicletelemetrysimulator**, replacing the login server value with your container registry login server value. The login server takes the form of **[container repository name].azurecr.io**.

    ![In the JSON editor the image repository property is highlighted showing the repository for the compiled image.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image199.png 'Image Repository property')

9. Sign in to Docker by entering the following command in the VS Code integrated terminal, using the Container Registry credentials and server information:

    ```Bash
    docker login -u <username>    -p <password>    <Login server>
    ```

10. To build the project, right-click the **module.json** file in the Explorer and select **Build IoT Edge Module Image**.

    ![The module.json file is right-clicked and the Build IoT Edge Module Image item is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image267.png 'Build IoT Edge module')

11. Select **amd64** as the platform of choice. This will create a Linux-based Docker image.

    ![amd64 is selected as the platform.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image91.png 'Select Platform')

12. To push the image to the Azure Container Registry, right-click the **module.json** file and select **Build and Push IoT Edge Module Image**.

    ![The module.json file is right-clicked and the Build and Push IoT Edge Module Image item is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image89.png 'Build and push IoT Edge module')

13. Once again, select **amd64** as the platform of choice. This will create a Linux-based Docker image.

    ![amd64 is selected as the platform.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image91.png 'Select Platform')

14. Watch the VS Code terminal window. You should see a success status when the build is complete. **Take note of the tag applied to your vehicle-telemetry-simulator image**. You will need to use this tag when you add the module to your IoT Edge device via the portal later on.

    ![In a terminal window, next to "Successfully tagged," the tag is value is highlighted.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image92.png 'VS Code terminal window showing a successful build')

### Task 4: Create the Azure Stream Analytics IoT Edge module

In this task, you will create a Stream Analytics job that filters vehicle telemetry data generated by the custom C\# module, and outputs only the most important data to potentially two different outputs in IoT Hub.

1. Navigate to the Azure Portal, <http://portal.azure.com>.

2. Select **+ Create a resource**, then type **stream analytics** into the search box on top. Select **Stream Analytics job** from the results.

    ![The search field in the Azure Portal is set to stream analytics.Stream Analytics job item is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image96.png 'Add stream analytics job')

3. Select the **Create** button on the Stream Analytics job overview blade.

4. On the New Stream Analytics job blade, specify the following configuration options:

    a. **Name**: Unique value for the Job name (ensure the green check mark appears).

    b. Specify your **Resource Group**, ensuring it's the same one in which your new components have been created.

    c. Select the same **location** as your Resource Group and other services. Please note: Currently, Azure Stream Analytics jobs on IoT Edge aren't supported in the West US 2 region.

    d. Select the **Edge** hosting environment.

    ![In the New Stream Analytics job form fields are set to the previously defined settings.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image97.png 'New Stream Analytics job blade')

5. Select **Create**.

6. In the created job, under **Job Topology**, select **Inputs**, and then select **+ Add stream input**, then select **Edge Hub**.

    ![The Add stream input button is highlighted, then the Edge Hub item is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image98.png 'Inputs blade')

7. Provide the following configuration in the New input blade:

    a. **Input alias**: VehicleTelemetry

    b. **Event-serialization format**: JSON

    c. **Encoding**: UTF-8

    d. **Compression**: None

    ![The input form is displayed with the previous values populated.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image99.png 'Edge Hub new input')

8. Select **Save**.

9. Under **Job Topology**, select **Outputs**, and then select **+ Add**, then select **Edge Hub**.

10. Provide the following configuration in the New output blade:

    a. **Input alias**: Alert

    b. **Event-serialization format**: JSON

    c. **Encoding**: UTF-8

    ![The new output form is displayed populated with the previous values.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image100.png 'Edge Hub new output')

11. Select **Save**.

12. Select **+ Add** under **Outputs** to create a new output that will trigger the route you created in IoT Hub earlier, that sends events filtered on the EngineAlert output, to the custom endpoint and on to the Service Bus Queue.

13. Provide the following configuration in the New output blade:

    a. **Input alias**: EngineAlert

    b. **Event-serialization format**: JSON

    c. **Encoding**: UTF-8

    ![Enter EngineAlert for the Output alias.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image101.png 'Edge Hub new output')

14. Select **Save**.
  
15. From the left-hand Stream Analytics menu, select the **Query** item from the Job Topology section. You will see the input and three outputs that you created. Select **Edit query** to the right of the displayed Query container.

    ![The Query menu item is selected from the left hand menu. The Edit query button is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image102.png 'Stream Analytics overview blade')

16. Create a step that averages the engine temperature and speed over a two second duration. Create another step that selects all telemetry data, including the average values from the previous step, and specifies the following anomalies as new fields:

    a. **enginetempanomaly**: When the average engine temperature is \>= 405 or \<= 15.

    b. **oilanomaly**: When the engine oil \<= 1.

    c. **aggressivedriving**: When the transmission gear position is in first, second, or third, and the brake pedal status is 1, the accelerator pedal position \>= 90, and the average speed is \>= 55.

17. Have the query output all fields from the anomalies step into the Alert output where aggressivedriving = 1 or enginetempanomaly = 1.

18. Have the query output all fields from the anomalies step where the enginetempanomaly = 1 and oilanomaly = 1.

19. Have the query output all fields from all the data into a "sink" that records all data.

20. Here is the completed query:

    ```sql
    WITH
    Averages AS (
    select
        AVG(engineTemperature) averageEngineTemperature,
        AVG(speed) averageSpeed
    FROM
        VehicleTelemetry TIMESTAMP BY [timestamp]
    GROUP BY
        TumblingWindow(Duration(second, 2))
    ),
    Anomalies AS (
    select
        t.vin,
        t.[timestamp],
        t.outsideTemperature,
        t.engineTemperature,
        a.averageEngineTemperature,
        t.speed,
        a.averageSpeed,
        t.fuel,
        t.engineoil,
        t.tirepressure,
        t.odometer,
        t.accelerator_pedal_position,
        t.parking_brake_status,
        t.headlamp_status,
        t.brake_pedal_status,
        t.transmission_gear_position,
        t.ignition_status,
        t.windshield_wiper_status,
        t.abs,
        t.mlDetectedAggressiveDriving,
        (case when a.averageEngineTemperature >= 405 OR a.averageEngineTemperature <= 15 then 1 else 0 end) as enginetempanomaly,
        (case when t.engineoil <= 1 then 1 else 0 end) as oilanomaly,
        (case when (t.transmission_gear_position = 'first' OR
            t.transmission_gear_position = 'second' OR
            t.transmission_gear_position = 'third') AND
            t.brake_pedal_status = 1 AND
            t.accelerator_pedal_position >= 90 AND
            a.averageSpeed >= 55 OR t.mlDetectedAggressiveDriving = 1 then 1 else 0 end)  as aggressivedriving
    from VehicleTelemetry t TIMESTAMP BY [timestamp]
    INNER JOIN Averages a ON DATEDIFF(second, t, a) BETWEEN 0 And 2
    )
    SELECT
        *
    INTO
        Alert
    FROM
        Anomalies
    where aggressivedriving = 1 OR enginetempanomaly = 1
    SELECT
        *
    INTO
        EngineAlert
    FROM
        Anomalies
    where enginetempanomaly = 1 AND oilanomaly = 1

    ```

21. To test the query with sample data, press the **Upload sample input** button in the bottom-right panel toolbar.

    ![The edit query form is displayed, beneath the query textbox, the Upload sample input button is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image103.png 'Upload sample input')

22. Use the browse button to select the **sample-vehicle-telemetry.json** file extracted to the Lab-files folder from the starter solution zip file you downloaded. This file contains 1000 JSON records of simulated vehicle telemetry.

    ![In the Upload input data blade, the browse button is highlighted and the sample-vehicle-telemetry.json file is displayed in the textbox.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image104.png 'Select sample input file')

23. Select **OK**.

24. Select **Test query** in the toolbar above the query.

    ![In the Query blade, the Test query button is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image105.png 'Test the analytics query with the sample data')

25. In the Outputs list, ensure Alert is selected. The test results should display that there are 85 rows.

    ![In the Results section a list of data is displayed, at the top of the list it shows a label indicating it is showing 85 rows form alert.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image106.png 'Test Results section')

26. **Save** the query.

### Task 5: Deploy custom modules to IoT Edge device

In this task, you will deploy the vehicle telemetry module, Stream Analytics module, and an IoT Edge Storage module to the IoT Edge device. All will be deployed simultaneously so you can register the module routes to send vehicle telemetry data to the Stream Analytics module, then send the filtered data upstream to IoT Hub as needed. The Vehicle Telemetry module will also send all telemetry data into IoT Edge Storage. The IoT Edge Storage module is responsible for synchronizing this data to a storage account in the cloud. The IoT Edge Storage module is intelligent, it can be configured to delete local blocks of data that have already been moved to the cloud, as well as recover from connection interruptions. IoT Edge Storage is ideal for a sometimes connected scenario, data will be held locally until a feasible internet connection is available to upload to the cloud.

1. Open your **IoT Hub**.

2. Select **IoT Edge** from the left-hand menu, then select your IoT Edge device to open the details page.

    ![In the IoT Hub, under Explorers, IoT Edge is selected. Under IoT Edge Devices, bus1 is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image107.png 'IoT Hub')

3. Select **Set Modules**.

    ![Set Modules button is selected in the Device Details menu.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image108.png 'Device Details blade')

4. In the **Container Registry Settings** add an entry with your container registry name, username and password.

    ![The IoT Edge Container Registry connection information form is displayed in the set modules screen. The container name, address, user name and password fields are highlighted and populated with values.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image204.png 'Container Registry connection information')

5. From the **IoT Edge Modules** section Select **Add** and choose **IoT Edge Module**.

    ![The Add button is selected, and the IoT Edge Module item is chosen in the Device Details blade.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image109.png 'Device Details blade')

6. Enter the following configuration values in the IoT Edge Module form:

    a. **Name**: VehicleTelemetry

    b. **Image URI**: The image URI path you specified when you created the Docker image and registered it in Azure Container Registry. Should be in the form of **\<your container registry address\>/vehicle-telemetry-simulator:0.0.1-amd64** (the tag should be the same as defined when your docker image was created in VS Code).

    c. **Restart Policy**: always

    d. **Desired Status**: running

    ![The add module form is displayed populated with the previous values](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image110.png 'IoT Edge Module blade')

7. Select **Add**.

8. From the **IoT Edge Modules** section Select **Add** and choose **Azure Stream Analytics Module**.

    ![The Add button is selected, and the Azure Stream Analytics module item highlighted in the IoT Edge Modules section.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image111.png 'Device Details blade')

9. Select your Azure subscription, then the Stream Analytics job you created in the previous task.

10. If you are missing a storage account setting, select the link to set it up.

    ![A message is displayed indicating the Edge job you have selected is missing Storage account settings, please set them up here. The here word is a link that is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image205.png "Missing Storage Account")

11. Once the Stream Analytics Job Storage Account Settings blade is opened, press the **Add storage account** button.

    a. **Storage Account Settings**: choose **Select storage account from your subscriptions**.

    b. **Subscription**: select your desired subscription.

    c. **Storage account**: select the storage account in which you created the **asa-container** blob earlier.

    d. **Container**: select **Use existing**, then select **asa-container**

    ![The Stream analytics job form is displayed with the fields set to the previously defined settings.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image112.png 'Edge deployment blade')

12. Select **Save**, then close the tab that was added to your browser to select the storage account.

13. Return to the Stream Analytics - Edge job blade, and press **Save**, this will publish the module.

14. Once published, copy the name of your Stream Analytics module.

    ![iot-edge-sa is highlighted in the deployment modules list.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image113.png 'Deployment modules list')

15. From the **IoT Edge Modules** section Select **Add** and choose **IoT Edge Module**.

    ![The Add button is selected, and the IoT Edge Module item is chosen in the Device Details blade.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image109.png 'Device Details blade')

16. Enter the following configuration values in the IoT Edge Module form:

    a. **Name**: AzureBlobStorageonIoTEdge

    b. **Image URI**: `mcr.microsoft.com/azure-blob-storage:latest`

    c. **Restart Policy**: always

    d. **Desired Status**: running

    ![The add module form is displayed populated with the previous values](media/new-iot-edge-storage-module.png 'IoT Edge Module blade')

17. Select the **Container Create Options** tab, enter the following:

    ```javascript
    {
    "Env":[
        "LOCAL_STORAGE_ACCOUNT_NAME=edgestorage",
        "LOCAL_STORAGE_ACCOUNT_KEY=pM8cWFj0L8h+VKRfE8Fy3tVVtdfOR4bCIzX8N/sDiK1X0znhu8iatFwVfjzwjedDKe5ln+2cI7wpy+2eO1vvQQ=="
    ],
    "HostConfig":{
        "Binds":[
            "/storage/containerdata:/blobroot"
        ],
        "PortBindings":{
        "11002/tcp":[{"HostPort":"11002"}]
        }
    }
    }
    ```

    ![The Update IoT Edge Module blade is shown with the Container Create Options tab selected with a textbox populated with the JSON based configuration above.](media/new-iot-edge-storage-model-container.png)

18. Select the **Module Twin Settings** tab.

    - Desired properties textbox - remember to replace the **cloudStorageConnectionString** value with your own:

        ```javascript
        {
            "deviceAutoDeleteProperties": {
                "deleteOn": false,
                "retainWhileUploading": true
            },
            "deviceToCloudUploadProperties": {
                "uploadOn": true,
                "uploadOrder": "OldestFirst",
                "cloudStorageConnectionString": "<Storage Account Connection String>",
                "storageContainersForUpload": {
                    "telemetry": {
                        "target": "telemetrysink"
                    }
                }
            },
            "deleteAfterUpload": true
        }
        ```

        ![The Update IoT Edge Module blade is shown with the Module Twin Settings tab selected with a textbox populated with the JSON based configuration above.](media/new-iot-edge-storage-model-twin.png)

19. Select **Add**.

20. Select **Next: Routes >** underneath the IoT Edge modules list.

21. In the **Routes** table add the following. Replace _{moduleName}_ with the Stream Analytics module name that you copied):

    | Name | Value |
    |------|---------|
    | alertsToCloud | FROM /messages/modules/{moduleName}/outputs/* INTO $upstream |
    | telemetryToAsa | FROM /messages/modules/VehicleTelemetry/outputs/* INTO BrokeredEndpoint("/modules/{moduleName}/inputs/VehicleTelemetry") |

    ![The routes are displayed for the bus1 modules as specified in the previous table above.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image114.png 'Set Modules, Routes page')

22. Select **Review + Create**.

23. In the Review screen, select **Create**.

24. After approximately 4 minutes, return to the device details page. You should see the three new modules running, along with the IoT Edge agent module and the IoT Edge hub.

    ![On the Deployed Modules tab, under Name, iot-edge-sa, azureblobstorageoniotedge, and VehicleTelemetry modules are called out.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image115.png 'Device Details page Deployed Modules tab')

25. Go back to your Bash shell that is connected to the Linux VM containing your IoT Edge device.

26. Execute the following to make sure all the modules are running in Docker:

    ```Bash
    sudo iotedge list
    ```

    ![Bash window displays the list of docker images running in the IoT Edge simulator VM.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image116.png 'Bash shell')

27. You should have five containers running at this point.

28. View the Stream Analytics module logs to see the telemetry it is reading, as well as any outputs it generates based on anomalies. You should see a large degree more vehicle telemetry feeding into the Stream Analytics module than what it sends out. This, of course, is by design. Replace {moduleName} with the Stream Analytics module name. Press <kbd>Ctrl/Cmd</kbd>+<kbd>c</kbd> to return to the command line.

    ```Bash
    sudo iotedge logs -f {moduleName}
    ```

    ![In the Output window, results from the Stream Analytics module logs display.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image206.png 'Output window')

31. View the generated data by viewing the log for the VehicleTelemetry module as follows. Press <kbd>Ctrl/Cmd</kbd>+<kbd>c</kbd> to return to the command line.

    ```Bash
    sudo iotedge logs -f VehicleTelemetry
    ```

32. Notice the log output as shown below. There are many "Sending message: \[VehicleTelemetry\]" events, and one output generated (highlighted). The output name is **alert**, matching one of the two outputs we created in the Stream Analytics module. The message content is sent to IoT Hub, including the additional fields added by the Stream Analytics query. In this case, the telemetry data is flagged as aggressive driving (aggressivedriving: 1).

    ![In the Bash window, results from the Vehicle Telemetry module logs display.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image207.png 'Bash window')

33. Leave the IoT Edge device running for the remainder of the lab.

## Exercise 5: Create an Azure Function to add critical engine alerts to the Service Bus Queue

As you remember, you created an Azure Service Bus Queue to hold messages flagged as critical engine alerts. This queue is to be fed messages from the IoT Hub that match a certain criteria. In this exercise, you will create the Azure Function that reads messages from the IoT Hub, identifies messages fitting the criteria and moves this information to the alert-q created earlier in this lab.

### Task 1: Create a new Function App

A Function app is a logical collection of functions on the Azure platform. Each Function app may have multiple functions contained within. Create a new Function App

*Tasks to complete:*

- Navigate to the Azure portal and create a new Function App and add it to your resource group.

*Exit criteria:*

- You have created a **Node.js** (version 12 or higher) Function App with the code publish option (**not** a Docker container).

### Task 2: Add new Function to process messages received by the IoT Hub

1. Open the Function App provisioned in the last task.

2. Underneath the Function App, select the **Functions** item, and press the **+ Add** button.

   ![The function app is expanded and the functions item is selected. The + Add button is selected in the functions window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image216.png 'Create new Function')

3. On the **New Function** blade, in the Templates search box, type IoT Hub, then select **IoT Hub (Event Hub)**.

   ![The Templates search box is displayed with IoT Hub typed in and the IoT Hub(Event Hub) template is selected from the list.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image217.png 'IoT Hub Template')

4. On the **New Function** blade **Details** tab, name the function **IoTHub_CriticalAlerts**, and enter **queuefuncconsumergroup** in the **Event Hub consumer group** textbox.

   ![The New Function blade is shown populated with the previous values.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image218.png)

5. Beneath the **Event Hub connection** field, press **New** and select **IoT Hub**, then select the IoT Hub being used for this lab. Ensure the **Endpoint** is set to **Events (built-in endpoint)**.

   ![The New Event Hub connection form is displayed with IoT Hub selected. The IoT Hub drop down is set to the IoT Hub from the remote monitoring solution, and the Endpoint is set to Events (built-in endpoint).](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image220.png 'Select IoT Hub Events Endpoint')

6. Press the **Create** button.

7. This function will be responsible for analyzing instant measurements of engine oil and engine temperature. The threshold for engine oil is to flag any values less than 20, and any engine temperatures greater than 400. Readings that are flagged are to be put into the Service bus **alert-q** that we created earlier in this lab.

8. In order to move flagged messages to the queue, we must declare a Service Bus output for our function. Open the **IoTHub_CriticalAlerts** function that we just created, and select **Integration** from the left menu.

   ![The Function left menu is displayed with the Integration item selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image223.png)

9. Next, select **+ Add output** from the **Outputs** node.

    ![The function Integration diagram is shown with the Add output link highlighted in the Outputs node.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image219.png)

10. In the **Create Output** blade, fill the form as follows:

    a. **Binding Type**: Select the **Azure Service Bus** item.

    b. **Message type**: Select **Service Bus Queue**.

    c. **Message parameter name**: Enter **engineAlertOutput**

    d. **Queue name**: Enter **alert-q**

    ![The Create Output blade is shown and is populated with the preceding values.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image224.png 'Define function output')

11. On the same form, select the **New** link beneath the **Service Bus Connection** field. Ensure **Service Bus** is selected, and then select the Service Bus Namespace that you created in [Exercise 2 Task 1: Create Service Bus queue](#task-1-create-service-bus-queue).
  
    ![The New Service Bus connection form is displayed populated with the preceding values.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image225.png)

12. Select **OK** to create the Output.

13. From the left menu, select **Code + Test**.

14. This brings up a window with the code for the **index.js** file. Replace the existing content with the following code:

    ```javascript
    module.exports = function (context, IoTHubMessages) {  
        IoTHubMessages.forEach(message => {
            if((message.engineoil && message.engineoil<20) || (message.engineTemperature && message.engineTemperature>400)){
                context.log("Engine Oil: "+ message.engineoil +" - Engine Temperature: "+ message.engineTemperature);
                context.bindings.engineAlertOutput = message;
            }
        });  
        context.done();
    };
    ```

15. Save the code, and watch the Logs for output.

    ![The Azure Function logs are displayed with two engine alerts highlighted in the output](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image228.png 'Function Console Output')

16. After a few minutes, verify the Service Bus queue has messages. Access the queue in the Azure portal, by selecting **All Resources**, and choosing the service bus namespace that was created earlier in this lab, then selecting **Queues** and **alert-q**.

    ![All resources is selected from the main left-hand menu. In the list of resources, the service bus namespace that we created earlier is selected. In the service bus namespace menu, the Queues item is selected. In the list of available queues, the alert-q is selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image229.png 'Access the Service Bus Queue')

17. You should see one or more messages ready for processing on the queue.

![Queue Details](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image230.png 'Queue Details')

## Exercise 6: Run a console app to view critical engine alerts from the Service Bus Queue

As you remember, you created an Azure Service Bus Queue to ingest messages flagged as critical engine alerts. This queue is to be fed messages from the IoT Hub that match a certain criteria. In this exercise, you will create the Azure that reads the Service Bus queue one-at-a-time so you can easily view the contents of the alerts.

**Duration:** 10 minutes

### Help references

|                                                               |                                                                                                                                             |
| ------------------------------------------------------------- | :-----------------------------------------------------------------------------------------------------------------------------------------: |
| **Description**                                               |                                                                  **Links**                                                                  |
| Use the Service Bus .NET SDK to receive messages from a queue | <https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-dotnet-get-started-with-queues#4-receive-messages-from-the-queue> |

### Task 1: Retrieve the Service Bus Queue Connection string

1. Once again, open your **Service Bus** instance in the Azure Portal.

2. Select **Shared access policies** from the left-hand menu.

3. Select the **RootManageSharedAccessKey** policy, then copy the **Primary Connection String** and save it for the next task.

    ![In the Service Bus blade, under Settings, Shared access policies is selected. Under Policy, RootManageSharedAccessKey is selected. The copy button is selected for the Primary Connection String.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image120.png 'Service Bus blade')

### Task 2: Configure and execute the ReadEngineAlerts solution in Visual Studio 2019

1. Browse to the Lab-files folder containing the extracted solution files for the lab.

2. Open **ReadEngineAlerts\\ReadEngineAlerts.sln** with Visual Studio 2019.

3. Open **Program.cs** from the Solution Explorer.

4. Locate the **connectionString** variable and replace {YOUR-CONNECTION-STRING} with the Service Bus connection string that you copied in the previous task.

    ![In Solution Explorer, Program.cs is selected, and the connectionString variable is circled.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image121.png 'Solution Explorer')

5. Right-click the **ReadEngineAlerts** project in Solution Explorer, then select **Start new instance** in the **Debug** submenu.

    ![In Solution Explorer, ReadEngineAlerts is selected, and from its right-click menu, Debug and Start new instance are selected.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image122.png 'Solution Explorer')

6. You should start to see critical alerts flowing in to the console window.

    ![Critical alerts display with varying colors in the Console window.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image123.png 'Console window')

7. Notice that the messages are flagged as anomalous. Meaning that there is a 1 in either the enginetempanomaly, oilanomaly, or aggressivedriving properties. This is due to the filter you defined within the Stream Analytics IoT Edge module.

8. You may close the console when you are finished reviewing the data.

## After the team challenge

**Duration:** 10 minutes

In this exercise, attendees will deprovision any Azure resources that were created in support of the lab.

### Task 1: Deprovision the accelerator through the website

1. Access the [Azure IoT Accelerators Site](https://www.azureiotsolutions.com/Accelerators).

2. Log in using your Azure Portal Credentials, then press the **My Solutions** button.

    ![The user avatar is selected indicating the need to sign in. The My Solutions menu item is selected from the top menu.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image236.png 'Sign in and access My Solutions')

3. Press the **iot-remote-monitoring** solution card.

    ![The IoT Remote Monitoring solution is listed as a deployed solution the launch button is crossed out as you need to select the information card and not the launch button in order to delete the accelerator.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image237.png 'Select the IoT Remote Monitoring solution')

4. Select the **Delete Solution** button. Deleting the Remote Monitoring Accelerator from here will ensure that all resources are deleted that were created in the process of the lab, including active directory entries, and the other resources that we included in the resource group.

    ![The remote monitoring details are displayed along with a delete button selected at the bottom of the screen.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image238.png 'Delete solution')

You should follow all steps provided *after* attending the team challenge.
