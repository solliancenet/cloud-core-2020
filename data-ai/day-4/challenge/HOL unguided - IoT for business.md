![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
IoT for business
</div>

<div class="MCWHeader2">
Hands-on lab unguided
</div>

<div class="MCWHeader3">
August 2018
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only, and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third-party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are the property of their respective owners.

**Contents** 

<!-- TOC -->

- [IoT for business hands-on lab unguided](#iot-for-business-hands-on-lab-unguided)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Exercise 1: Azure data, storage, and serverless environment setup](#exercise-1-azure-data-storage-and-serverless-environment-setup)
        - [Help references](#help-references)
        - [Task 1: Execute CLI commands](#task-1-execute-cli-commands)
            - [Tasks to complete](#tasks-to-complete)
            - [Exit criteria](#exit-criteria)
        - [Task 2: Open new web application and stop running device simulation](#task-2-open-new-web-application-and-stop-running-device-simulation)
            - [Tasks to complete](#tasks-to-complete-1)
            - [Exit criteria](#exit-criteria-1)
    - [Exercise 2: Provision additional Azure services](#exercise-2-provision-additional-azure-services)
        - [Help references](#help-references-1)
        - [Task 1: Create Service Bus queue](#task-1-create-service-bus-queue)
            - [Tasks to complete](#tasks-to-complete-2)
            - [Exit criteria](#exit-criteria-2)
        - [Task 2: Add consumer group to IoT Hub](#task-2-add-consumer-group-to-iot-hub)
            - [Tasks to complete](#tasks-to-complete-3)
            - [Exit criteria](#exit-criteria-3)
        - [Task 3: Add custom endpoint to IoT Hub](#task-3-add-custom-endpoint-to-iot-hub)
            - [Tasks to complete](#tasks-to-complete-4)
            - [Exit criteria](#exit-criteria-4)
        - [Task 4: Create custom IoT Hub route](#task-4-create-custom-iot-hub-route)
            - [Tasks to complete](#tasks-to-complete-5)
            - [Exit criteria](#exit-criteria-5)
        - [Task 5: Create Azure Time Series Insights instance](#task-5-create-azure-time-series-insights-instance)
            - [Tasks to complete](#tasks-to-complete-6)
            - [Exit criteria](#exit-criteria-6)
        - [Task 6: Provision Azure Container Registry](#task-6-provision-azure-container-registry)
            - [Tasks to complete](#tasks-to-complete-7)
            - [Exit criteria](#exit-criteria-7)
        - [Task 7: Provision new Linux virtual machine to run the IoT Edge device](#task-7-provision-new-linux-virtual-machine-to-run-the-iot-edge-device)
            - [Tasks to complete](#tasks-to-complete-8)
            - [Exit criteria](#exit-criteria-8)
    - [Exercise 3: Create bus and traffic light simulated devices, and add alerts and filters](#exercise-3-create-bus-and-traffic-light-simulated-devices-and-add-alerts-and-filters)
        - [Help references](#help-references-2)
        - [Task 1: Configure the SimulationAgent and WebService projects to run locally](#task-1-configure-the-simulationagent-and-webservice-projects-to-run-locally)
            - [Tasks to complete](#tasks-to-complete-9)
            - [Exit criteria](#exit-criteria-9)
        - [Task 2: Finish configuring the simulated IoT device models and scripts](#task-2-finish-configuring-the-simulated-iot-device-models-and-scripts)
            - [Tasks to complete](#tasks-to-complete-10)
            - [Exit criteria](#exit-criteria-10)
        - [Task 3: Explore the remaining files to understand what is happening](#task-3-explore-the-remaining-files-to-understand-what-is-happening)
            - [Tasks to complete](#tasks-to-complete-11)
        - [Task 4: Configure and run the Storage Adapter project](#task-4-configure-and-run-the-storage-adapter-project)
            - [Tasks to complete](#tasks-to-complete-12)
            - [Exit criteria](#exit-criteria-11)
        - [Task 5: Run the Simulator web app and create a new simulation](#task-5-run-the-simulator-web-app-and-create-a-new-simulation)
            - [Tasks to complete](#tasks-to-complete-13)
            - [Exit criteria](#exit-criteria-12)
        - [Task 6: Run the device simulation agent locally](#task-6-run-the-device-simulation-agent-locally)
            - [Tasks to complete](#tasks-to-complete-14)
            - [Exit criteria](#exit-criteria-13)
        - [Task 7: Create alerts and filters in the monitoring web app](#task-7-create-alerts-and-filters-in-the-monitoring-web-app)
            - [Tasks to complete](#tasks-to-complete-15)
            - [Exit criteria](#exit-criteria-14)
        - [Task 8: Send jobs to IoT devices](#task-8-send-jobs-to-iot-devices)
            - [Tasks to complete](#tasks-to-complete-16)
            - [Exit criteria](#exit-criteria-15)
    - [Exercise 4: Create IoT Edge device and custom modules](#exercise-4-create-iot-edge-device-and-custom-modules)
        - [Help references](#help-references-3)
        - [Task 1: Add a new IoT Edge device](#task-1-add-a-new-iot-edge-device)
            - [Tasks to complete](#tasks-to-complete-17)
            - [Exit criteria](#exit-criteria-16)
        - [Task 2: Install and start the IoT Edge runtime](#task-2-install-and-start-the-iot-edge-runtime)
            - [Tasks to complete](#tasks-to-complete-18)
            - [Exit criteria](#exit-criteria-17)
        - [Task 3: Create and deploy the custom C\# IoT Edge module for vehicle telemetry](#task-3-create-and-deploy-the-custom-c\-iot-edge-module-for-vehicle-telemetry)
            - [Tasks to complete](#tasks-to-complete-19)
            - [Exit criteria](#exit-criteria-18)
        - [Task 4: Create the Azure Stream Analytics IoT Edge module](#task-4-create-the-azure-stream-analytics-iot-edge-module)
            - [Tasks to complete](#tasks-to-complete-20)
            - [Exit criteria](#exit-criteria-19)
        - [Task 5: Deploy the custom modules to IoT Edge device](#task-5-deploy-the-custom-modules-to-iot-edge-device)
            - [Tasks to complete](#tasks-to-complete-21)
            - [Exit criteria](#exit-criteria-20)
    - [Exercise 5: Run a console app to view critical engine alerts from the Service Bus Queue](#exercise-5-run-a-console-app-to-view-critical-engine-alerts-from-the-service-bus-queue)
        - [Help references](#help-references-4)
        - [Task 1: Make sure the Service Bus Queue is receiving messages](#task-1-make-sure-the-service-bus-queue-is-receiving-messages)
            - [Tasks to complete](#tasks-to-complete-22)
            - [Exit criteria](#exit-criteria-21)
        - [Task 2: Configure and execute the ReadEngineAlerts solution in Visual Studio 2017](#task-2-configure-and-execute-the-readenginealerts-solution-in-visual-studio-2017)
            - [Tasks to complete](#tasks-to-complete-23)
            - [Exit criteria](#exit-criteria-22)
    - [Exercise 6: Create Azure Function App to ingest critical engine alerts and store them in Cosmos DB](#exercise-6-create-azure-function-app-to-ingest-critical-engine-alerts-and-store-them-in-cosmos-db)
        - [Help references](#help-references-5)
        - [Task 1: Create new Cosmos DB collection](#task-1-create-new-cosmos-db-collection)
            - [Tasks to complete](#tasks-to-complete-24)
            - [Exit criteria](#exit-criteria-23)
        - [Task 2: Create a new Function App](#task-2-create-a-new-function-app)
            - [Tasks to complete](#tasks-to-complete-25)
            - [Exit criteria](#exit-criteria-24)
        - [Task 3: Create a new Function](#task-3-create-a-new-function)
            - [Tasks to complete](#tasks-to-complete-26)
            - [Exit criteria](#exit-criteria-25)
        - [Task 4: Add an Azure Cosmos DB output to the messages collection](#task-4-add-an-azure-cosmos-db-output-to-the-messages-collection)
            - [Tasks to complete](#tasks-to-complete-27)
            - [Exit criteria](#exit-criteria-26)
        - [Task 5: Add an Azure Cosmos DB output to the critical-alerts collection](#task-5-add-an-azure-cosmos-db-output-to-the-critical-alerts-collection)
            - [Tasks to complete](#tasks-to-complete-28)
            - [Exit criteria](#exit-criteria-27)
        - [Task 6: Modify the function code](#task-6-modify-the-function-code)
            - [Tasks to complete](#tasks-to-complete-29)
            - [Exit criteria](#exit-criteria-28)
        - [Task 7: View data stored by the function in Azure Cosmos DB](#task-7-view-data-stored-by-the-function-in-azure-cosmos-db)
            - [Tasks to complete](#tasks-to-complete-30)
            - [Exit criteria](#exit-criteria-29)
        - [Task 8: View the critical engine alerts in the IoT Remote Monitoring web interface](#task-8-view-the-critical-engine-alerts-in-the-iot-remote-monitoring-web-interface)
            - [Tasks to complete](#tasks-to-complete-31)
            - [Exit criteria](#exit-criteria-30)
        - [Task 9: Add tag to Edge's device twin and create new filter from it](#task-9-add-tag-to-edges-device-twin-and-create-new-filter-from-it)
            - [Tasks to complete](#tasks-to-complete-32)
            - [Exit criteria](#exit-criteria-31)
    - [Exercise 7: View all data in Azure Time Series Insights](#exercise-7-view-all-data-in-azure-time-series-insights)
        - [Help references](#help-references-6)
        - [Task 1: Add your account as a Contributor to the Data Access Policies](#task-1-add-your-account-as-a-contributor-to-the-data-access-policies)
            - [Tasks to complete](#tasks-to-complete-33)
            - [Exit criteria](#exit-criteria-32)
        - [Task 2: Go to the Time Series Insights environment and use the data explorer](#task-2-go-to-the-time-series-insights-environment-and-use-the-data-explorer)
            - [Tasks to complete](#tasks-to-complete-34)
            - [Exit criteria](#exit-criteria-33)
        - [Task 3: View the simulated and IoT Edge bus data side-by-side](#task-3-view-the-simulated-and-iot-edge-bus-data-side-by-side)
            - [Tasks to complete](#tasks-to-complete-35)
            - [Exit criteria](#exit-criteria-34)
        - [Task 4: Use Perspective View to create a simultaneous view of up to four unique queries](#task-4-use-perspective-view-to-create-a-simultaneous-view-of-up-to-four-unique-queries)
            - [Tasks to complete](#tasks-to-complete-36)
            - [Exit criteria](#exit-criteria-35)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Task 1: Delete the Resource Group in which you placed your Azure resources.](#task-1-delete-the-resource-group-in-which-you-placed-your-azure-resources)

<!-- /TOC -->

# IoT for business hands-on lab unguided

## Abstract and learning objectives 

In this hands-on-lab, you will build an end-to-end smart city solution, beginning with IoT Edge devices deployed with modules that you create which intelligently filters vehicle telemetry data for anomalies and transmits the related data to IoT Hub. IoT Hub is responsible for managing IoT devices and facilitating two-way communication between those devices and Azure services. The telemetry data will be stored in Time Series Insights, and all critical data will also flow through a custom IoT Hub endpoint that routes critical alerts to a Service Bus Queue for separate processing and storage. You will deploy and configure a custom web app that displays all IoT data on a map and displays alerts based on preconfigured rules for each type of IoT device. You will also use this custom web app to configure IoT devices and send control messages to them via IoT Hub.

At the end of this hands-on lab, you will be better able to build an end-to-end IoT solution that processes and analyzes data both in the field and in the cloud.

## Overview

The IoT for business hands-on lab is an exercise that will challenge you to implement an end-to-end scenario using a supplied sample that is based on IoT Hub, IoT Edge devices, Cosmos DB, Time Series Insights, Service Bus, and related Azure services. The hands-on lab can be implemented on your own, but it is highly recommended to pair up with other members at the lab to model a real-world experience and to allow each member to share their expertise for the overall solution.

## Solution architecture

![The Solution Architecture diagram is described in the text following the diagram.](images/Hands-onLabunguided-IoTforbusinessimages/media/image2.png "Solution Architecture diagram")

The solution begins with an IoT Edge Device that would be installed on each city bus, which is responsible for reading the vehicle telemetry from the bus, such as speed, fuel level, oil level, engine temperature, etc., generated by custom C# module. A Stream Analytics module is loaded on the IoT Edge device to filter the vehicle telemetry data so that only anomalies are sent to IoT Hub. A GPS IoT device is separately added to the bus to periodically send location and speed data to IoT Hub. An IoT device is added to various traffic lights to send maintenance-related telemetry, such as voltage readings and whether a light is no longer functional. It is registered as a device in IoT Hub, including properties such as its longitude and latitude and serial number. It can receive cloud-to-device messages through IoT Hub, allowing upstream services to send updates like the timing of its lights based on traffic congestion information. The GPS IoT and traffic light devices are simulated using the IoT Remote Monitoring solution device simulator. An additional consumer group is added to IoT Hub's messages/events endpoint, allowing a Time Series Insights instance and an EventProcessor running on a microservice to simultaneously read the incoming data. Time Series Insights is used to store the raw time series data and provides advanced filtering, custom ad-hoc queries, and visualizations that can overlay data from several classes of IoT devices. A custom endpoint is created in Azure IoT Hub that is used to route filtered bus engine-related critical messages to a Service Bus Queue. An Azure Function is triggered on items being added to the queue, and outputs the critical alerts to a new collection in Cosmos DB. The EventProcessor is used to process incoming messages from IoT Hub to extract GPS and traffic light data, and store the data and any triggered alerts in Cosmos DB. The web app, which is part of the IoT Remote Monitoring solution, connects to Cosmos DB and displays the IoT data on a map, provides real-time charts, and allows you to manage alert rules and device control messages. It is also used to provision new IoT devices and send manual cloud-to-device messages through IoT Hub.

## Requirements

-   Microsoft Azure subscription (non-Microsoft subscription) where you are at least a co-administrator.

-   **Global Administrator role** for Azure AD within your subscription.

-   Local machine or a virtual machine configured with (**complete the day before the lab!**):

    -   Visual Studio Code version 1.19.2 or higher

        -   <https://code.visualstudio.com/>

    -   Azure IoT Edge extension for Visual Studio Code

        -   <https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge>

    -   C\# for Visual Studio Code (powered by OmniSharp) extension

        -   <https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp>

    -   Docker on the same computer that has Visual Studio Code (Community Edition (CE) is sufficient)

        -   <https://docs.docker.com/engine/installation/>

    -   .NET Core 2.0 SDK

        -   <https://www.microsoft.com/net/core#windowscmd>

    -   Visual Studio Community 2017 or greater, version 15.4 or higher

        -   <https://www.visualstudio.com/vs/>

    -   Azure development workload for Visual Studio 2017

        -   <https://docs.microsoft.com/azure/azure-functions/functions-develop-vs#prerequisites>

    -   .NET desktop development workload for Visual Studio 2017

    -   ASP.NET and web development workload for Visual Studio 2017

    -   Node.js (install using either the 32-bit or 64-bit Windows Installer (.msi) option)

        -   <https://nodejs.org/en/download/>

    -   Postman app

        -   <https://www.getpostman.com/apps>

    -   Bash client (such as Git Bash or Bash on Ubuntu for Windows)

        -   Instructions for installing the Windows Subsystem for Linux for using Bash: <https://docs.microsoft.com/en-us/windows/wsl/install-win10>

## Exercise 1: Azure data, storage, and serverless environment setup

**Duration:** 30 minutes

In this exercise, you will execute a series of command-line scripts to provision the components used by the IoT Remote Monitoring solution. This includes an Azure Resource Group, IoT Hub, Azure Storage account, App Service for hosting the monitoring web application, Virtual Machine and related components that host the Docker-based microservices responsible for creating, capturing, and processing IoT device messages, and a Cosmos DB instance for storing reference, alert, and telemetry data.

### Help references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
|   Remote Monitoring preconfigured solution with Azure IoT  | <https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet>     |

### Task 1: Execute CLI commands

In this task, you will navigate to the **azure-iot-pcs-remote-monitoring-dotnet\\cli** subfolder within your Hands-on Lab\Lab-files folder containing the solution files and execute commands to run a basic deployment of the Remote Monitoring solution.

#### Tasks to complete

-   Restore npm packages and execute a basic deployment.

#### Exit criteria 

-   Save the resource group name, which is where you will deploy the remaining Azure services for this lab.

-   Copy the monitoring web app URL.

### Task 2: Open new web application and stop running device simulation

Navigate to the monitoring web app using the URL copied in the previous task. Stop the current device simulation, since you will be creating your own.

#### Tasks to complete

-   Navigate to the monitoring web app and stop the current device simulation.

#### Exit criteria 

-   The device simulation is stopped.

-   You have navigated through the site to take note of its features.

## Exercise 2: Provision additional Azure services

**Duration:** 45 minutes

Explore the components automatically provisioned by the CLI scripts in the previous exercise. Use the Azure portal to create services that will be used by the overall IoT solution environment. First, you will create an additional consumer group on the default messages/events IoT Hub endpoint. This will allow us to add more readers that can read the massive amounts of incoming IoT data being ingested by IoT Hub. This will help prevent conflicts from multiple readers attempting to modify the checkpoints and offsets of the partitions holding the messages. Next, you will add a custom endpoint and route to IoT Hub specifically for engine alerts arriving from your IoT Edge device that you will create later. Finally, you will provision a new Azure Time Series Insights instance to collect, filter, and display all-time series data flowing in from your simulated IoT devices as well as Edge device.

### Help references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
| Remote Monitoring preconfigured solution with Azure IoT  | <https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet>     |
| Routing messages with IoT Hub     | <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-csharp-csharp-process-d2c>     | 
| Use message routes and custom endpoints for device-to-cloud messages | <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-messages-read-custom>     | 
| What is Azure Time Series Insights?    | <https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-overview>    | 


### Task 1: Create Service Bus queue

In this task, you will provision a new Service Bus queue that will be used for routing special bus engine-related critical alerts. Afterward, you will create a custom endpoint on your IoT Hub service that will be used to route filtered messages to this queue.

#### Tasks to complete

-   Create a Service Bus namespace with the standard pricing tier, in the Azure portal.

-   Create a queue:

    -   **Name**: alert-q

    -   **Max queue size**: 1 GB

    -   Leave the remaining values at their defaults. It is important to note that you must **not** enable duplicate detection or sessions on queues that will be used as custom IoT Hub service endpoints. These settings will cause unexpected results and failures if applied.

#### Exit criteria 

-   Save the Service Bus RootManagerSharedAccessKey access policy primary connection string.

### Task 2: Add consumer group to IoT Hub

In this task, you will add a new consumer group to the default messages/events IoT Hub endpoint, for the Time Series Insights service to use.

#### Tasks to complete

-   Add a consumer group named **timeseries** to the built-in Events endpoint.

#### Exit criteria 

-   You have an IoT Hub consumer group created for Time Series Insights.

### Task 3: Add custom endpoint to IoT Hub

IoT Hub allows you to add up to 10 custom endpoints. These custom endpoints allow you to differentiate between messages arriving to the hub by filtering those messages in custom-defined routes, and then directing them to either your custom endpoints or a built-in one, such as Events. In this task, you will create a custom endpoint that sends messages routed to it, directly to the Service Bus queue you created. This will allow us to process critical alerts differently from standard telemetry, opening up a new pipeline to which different business rules and actions can be applied.

#### Tasks to complete

-   Add a new custom endpoint to IoT Hub with the following parameters:

    -   **Name**: CriticalQueue

    -   **Endpoint type**: Service Bus Queue

    -   **Service Bus namespace**: select your new Service Bus namespace

    -   **Service Bus queue**: alert-q

#### Exit criteria 

-   You have a custom IoT Hub endpoint for sending critical bus engine alert messages to your Service Bus Queue.

### Task 4: Create custom IoT Hub route

In this task, you will create a new routing rule to send bus engine alert messages to the Service Bus queue, by way of your custom endpoint.

#### Tasks to complete

-   Create a new route with the following parameters:

    -   **Name**: CriticalMessageRule

    -   **Data source**: Device Messages

    -   **Endpoint**: Select your **CriticalQueue** custom endpoint.

    -   **Enable Rule**: On

    -   **Query string**: OutputName=\"enginealert\"

    -   The OutputName is provided by the Stream Analytics module we will create for the IoT Edge device. The "enginealert" value is one of two outputs you will add to the Stream Analytics module. This is what allows you to send specific events to this endpoint, while sending all events (including the ones sent here) to the default IoT Hub endpoint.

#### Exit criteria 

-   You have a routing rule for sending bus engine alert messages to your Service Bus Queue, through the custom endpoint you created in the previous task.

### Task 5: Create Azure Time Series Insights instance

Azure Time Series Insights is the first fully managed time series database on the Azure platform. It was developed primarily with high volume IoT data in mind, where having a single location in which you can quickly view this information and derive insights on it is typically no small feat. Although the IoT Remote Monitoring solution you provisioned stores its simulated device data in Cosmos DB, you will be able to ingest that same data into Time Series Insights, along with data generated by your IoT Edge device. This is because all data flows through IoT Hub as the initial point of ingress. You created a new consumer group on the Events endpoint specifically for Time Series Insights to be able to simultaneously read and store the same data that will land in Cosmos DB, as well as the added Edge data. After creating the Time Series Insights instance, you will configure it to use the IoT Hub consumer group as an input.

#### Tasks to complete

-   Create a new Azure Time Series Insights instance through the portal.

-   Add an event source that points to your IoT Hub and uses the **timeseries** consumer group you created.

#### Exit criteria 

-   You have a new Time Series Insights instance with an event source that accesses device data through the *timeseries* IoT Hub consumer group.

### Task 6: Provision Container Registry

IoT Edge devices use one or more modules to perform a series of actions locally on the device before sending data up to the cloud. Modules include custom modules written in a language like C\#, Azure Stream Analytics that runs on the device, Azure Machine Learning, and Azure Functions. Each of these modules is hosted within a Docker container. We will be creating two modules for the IoT Edge device: a custom C\# module, and an Azure Stream Analytics module. In both cases, you will be creating a container image from the files. The images are then pushed to a registry that stores and manages them. The final step is to deploy the images from the registry onto your IoT Edge devices. Two popular Docker registry services available in the cloud are Container Registry and Docker Hub. We will be using Container Registry to manage and deploy the IoT Edge modules.

#### Tasks to complete

-   Provision a Container Registry through the portal.

#### Exit criteria 

-   Copy the **Login server**, **Username**, and **Password** values and save them for later.

### Task 7: Provision new Linux virtual machine to run the IoT Edge device

In this task, you will provision a new Linux virtual machine that will be used to run the IoT Edge device, using an Azure quickstart template for Ubuntu pre-installed with Docker. Be certain to select Ubuntu OS Version 16.04.0-LTS or later.

#### Tasks to complete

-   Provision a new Linux virtual machine with Docker, using this template: <https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fdocker-simple-on-ubuntu%2Fazuredeploy.json>.

#### Exit criteria 

-   Save the admin password for later.

-   Ubuntu OS Version is **16.04.0-LTS** or later.


## Exercise 3: Create bus and traffic light simulated devices, and add alerts and filters

**Duration:** 60 minutes

The IoT Remote Monitoring solution allows you to provision and collect telemetry from both simulated and real devices. As part of the process, telemetry schema information is applied to the device's twin through its reported properties. The properties are read as the microservice that's executing the EventProcessor processes incoming messages from IoT Hub. The telemetry metadata is used to write the data to Cosmos DB, then used again later by the web app to extract and display the data on the chart and map. The metadata is also used to define cloud-to-device messages and actions that can be performed on the device. The web app uses this information to send control messages to the devices. This metadata is added to simulated and non-simulated devices alike.

In this exercise, you will define metadata for new device types that will be provisioned, and whose telemetry will be simulated by the solution. Each new device type will have a state script to generate telemetry that changes the device's state, whether speed, location, voltage, or other data that relates to the device. In addition, you will define cloud-to-device messages and actions for the new device types. Then, you will create and run a new simulation locally, using a Visual Studio solution. Finally, you will create new alerts and filters through the web app interface.

We have created the following files for you, located within the device-simulation project (/azure-iot-pcs-remote-monitoring-dotnet/device-simulation/Services/data/devicemodels):

-   **Device models**:

    -   bus-01.json

    -   bus-02.json

    -   trafficlight-01.json

    -   trafficlight-02.json

-   **Scripts (/scripts subfolder)**:

    -   bus-01-state.js

    -   bus-02-state.js

    -   trafficlight-01-state.js

    -   trafficlight-02-state.js

    -   DecreaseTiming-method.js

    -   IncreaseTiming-method.js

You will need to finish configuring these files for the simulator.

### Help references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
|  IoT Remote Monitoring device models specs  |  <https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models>  |
|  Creating and managing simulations  | <https://github.com/Azure/device-simulation-dotnet/wiki/%5BAPI-Specifications%5D-Simulations>  |
|  Use rules to detect issues   |  <https://docs.microsoft.com/en-us/azure/iot-suite/iot-suite-remote-monitoring-automate> |
|  Customize the device simulator microservice | <https://docs.microsoft.com/en-us/azure/iot-suite/iot-suite-remote-monitoring-test> | 
|  IoT Remote Monitoring solution architecture | <https://docs.microsoft.com/en-us/azure/iot-suite/iot-suite-remote-monitoring-sample-walkthrough> |

### Task 1: Configure the SimulationAgent and WebService projects to run locally

In this task, you will open the device-simulation solution in Visual Studio 2017 and configure the SimulationAgent and WebService projects to run locally.

#### Tasks to complete

-   Open the device-simulation solution in the following location: \[your-IoTLab-folder\]\\azure-iot-pcs-remote-monitoring-dotnet\\device-simulation\\device-simulation.sln.

-   Modify the SimulationAgent project properties and set the PCS\_IOTHUB\_CONNSTRING Environment Variable value, replacing "EnterHubConnString" with the IoT Hub connection string you copied earlier.

    -   Set the Target Framework to .NET Core 2.0.

-   Set the same Environment Variable in the WebService project.

    -   Set the Target Framework to .NET Core 2.0.

#### Exit criteria

-   Both projects have the PCS\_IOTHUB\_CONNSTRING Environment Variable configured with the IoT Hub connection string.

### Task 2: Finish configuring the simulated IoT device models and scripts

In this task, you will finish configuring the device models we have provided for you.

#### Tasks to complete

-   With the device-simulation solution still open, use Solution Explorer to expand the Services project. Next, open **bus-01.json** located under data\\devicemodels.

    -   Set the following Simulation.InitialState values. These are the parameters used at the start of the device simulation:

        -   **latitude**: 40.755086

        -   **longitude**: -73.984165

        -   **fuellevel**: 70.0

        -   **speed**: 30.0

        -   **vin**: Y3J9PV9TN36A4DUB9

    -   Set the following Properties:

        -   **Type**: Bus

        -   **Location**: Manhattan

-   There are two Telemetry schemas set for this bus. The first one should send telemetry every 10 seconds, while the other one should have an interval of one minute. Complete the Telemetry values according to the following specifications:

    -   Telemetry \#1

    -   MessageSchema.Fields:

        -   **latitude**: double

        -   **longitude**: double

        -   **speed**: double

        -   **speed\_unit**: text

    -   Telemetry \#2:

    -   MessageTemplate (use the MessageTemplate value of the first telemetry as a guide): Include the fuellevel and fuellevel\_unit fields.

    -   MessageSchema.Fields:

        -   **fuellevel**: double

        -   **fuellevel\_unit**: text

-   Open **bus-02.json**.

    -   Set the following Simulation.InitialState values. These are the parameters used at the start of the device simulation:

        -   **latitude**: 40.693935

        -   **longitude**: -73.952279

        -   **fuellevel**: 53.0

        -   **speed**: 42.0

        -   **vin**: 2K0H7PNZY0RSFQ033

    -   Set the following Properties:

        -   **Type**: Bus

        -   **Location**: Brooklyn

    -   There are two Telemetry schemas set for this bus. The first one should send telemetry every 12 seconds, while the other one should have an interval of 55 seconds. Complete the Telemetry values according to the following specifications:

    -   Telemetry \#1:

    -   MessageSchema.Fields:

        -   **latitude**: double

        -   **longitude**: double

        -   **speed**: double

        -   **speed\_unit**: text

        -   **vin**: text

    -   Telemetry \#2:

    -   MessageTemplate (use the MessageTemplate value of the first telemetry as a guide): Include the fuellevel and fuellevel\_unit fields.

    -   MessageSchema.Fields:

        -   **fuellevel**: double

        -   **fuellevel\_unit**: text

-   Open **trafficlight-01.json**.

    -   Add the following **CloudToDeviceMethods**:

        -   IncreaseTiming

            -   **Type**: javascript

            -   **Path**: IncreaseTiming-method.js

        -   DecreaseTiming

            -   **Type**: javascript

            -   **Path**: DecreaseTiming-method.js

-   Open **trafficlight-02.json**. Add the [same]{.underline} **CloudToDeviceMethods** that you added to trafficlight-01.json.

-   In Solution Explorer, expand the scripts subfolder (Services\\data\\devicemodels\\scripts), then open **bus-01-state.js**.

-   Scroll down to the **main** function and complete the following lines of code:

    -   Find TODO: 1 and complete the line of code underneath to set the state.speed value to a random double value with an average of 30, \~40%, a minimum value of 0 and a maximum of 80.

    -   Find TODO: 2 and complete the line of code underneath to set the state.fuellevel value to a random double value with an average of 70, \~25%, a minimum value of 2 and a maximum of 80.

-   Open **bus-02-state.js**.

    -   Scroll down to the **main** function and complete the following lines of code:

        -   Find TODO: 3 and complete the line of code underneath to set the state.speed value to a random double value with an average of 42, \~50%, a minimum value of 0 and a maximum of 80.

        -   Find TODO: 4 and complete the line of code underneath to set the state.fuellevel value to a random double value with an average of 53, \~25%, a minimum value of 2 and a maximum of 80.

#### Exit criteria

-   Open trafficlight-01-state.js and observe how it is altering values over time. You will see that it is varying the light state (red, green, yellow), as well as the voltage. If you look at trafficlight-02-state.js, you will notice that its voltage range is significantly higher than for the first traffic light. When we create an alert, this will most likely be the light that triggers it.

-   Open DecreaseTiming-method.js and see how it is used as an action method to reset the timing state of a traffic light by decreasing the timing 15 seconds at a time. This is executed by a cloud-to-device call from the monitoring web app.

### Task 3: Explore the remaining files to understand what is happening

Below is a table containing file paths and an explanation of what each does in the simulator. There are a few key things to point out so that you know how the Service SDK for Azure IoT Devices can be used to programmatically manage devices.

#### Tasks to complete

-   With the device-simulation solution still open in Visual Studio, look at each of the following files and descriptions to understand how things work:

    - Visual Studio Project: Services
        - File Path: Devices.cs                            
        - Description: GetAsync method (line 102) accepts a Device Id and uses it to retrieve the device details from IoT Hub, using the Service SDK's RegistryManager. It will optionally retrieve the device twin which can be used to view the current twin properties and update their values. CreateAsync method (line 156) is used to provision a new IoT Device, using the RegistryManager. It also creates a new device twin containing the IsSimulated tag. This is how the IoT Monitor app can differentiate between simulated and physical devices.    


    - Visual Studio Project: Services
        - File Path: DeviceClient.cs
        - Description: SendMessageAsync method (line 100) constructs a new Message object that it will send to IoT Hub. It includes the event message properties to include the message content type (JSON), and the schema name, as defined in the device model scripts you edited earlier. The microservice running the IoT Hub EventProcessor will look for these values before processing messages and saving them to Cosmos DB. 

    - Visual Studio Project: SimulationAgent      
        - File Path: Simulation\\DeviceActor.cs             
        - Description: Each device is assigned an instance of the DeviceActor class. This class manages the following state machine flow (as shown within the MoveNext method
            - Connect to IoT Hub
            - Bootstrap the device to retrieve it, create if necessary, and update the device twin state 
            - Update the device state using the state scripts we created, in order to send telemetry
            - Sends telemetry using the message template provided, as seen in the bus and traffic light device model scripts you edited earlier. Uses the DeviceClient class to send the message through the Device SDK for Azure IoT Devices |

    - Visual Studio Project: WebService
        - File Path: v1\\Controllers\\SimulationsController 
        - Description: This web API controller contains REST methods that allow you to retrieve, insert, update, and delete device simulations. The simulation values are updated in Cosmos DB by way of the Simulations service (Services project -- Simulations.cs), which uses the running pcs-storage-adapter service to modify the values in Cosmos DB. We will be using this next. 


### Task 4: Configure and run the Storage Adapter project

The Storage Adapter project (pcs-storage-adapter) is another microservice that constantly runs and provides REST-based endpoints to manage simple key/value data in Cosmos DB. It is used by several services, including the web service within the device-simulator project, as seen in the previous task. This needs to be configured, then executed to run before creating and running simulations on the new devices locally.

#### Tasks to complete

-   Browse and open the storage-adapter solution in the following location: \[your-IoTLab-folder\]\\azure-iot-pcs-remote-monitoring-dotnet\\storage-adapter\\pcs-storage-adapter.sln.

-   Modify the WebService project properties and set the PCS\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING Environment Variable value, replacing "..." with the Cosmos DB primary connection string.

    -   Set the Target Framework to .NET Core 2.0.

-   Run the WebService project. It will open in a new browser window with a status.

#### Exit criteria

-   The status response on the web page should show the service is OK.

-   Leave the project running in debug mode for the duration of the lab.

### Task 5: Run the Simulator web app and create a new simulation

In this task, you will run the Simulator web app locally and send REST-based commands to it to delete the existing simulation and define a new one using only the devices we want to simulate for the lab, including the new device types.

#### Tasks to complete

-   Debug the WebService project within the device-simulation solution in Visual Studio.

-   Use Postman to send a GET request to <http://localhost:9003/v1/simulations/1> so you can see the current IoT device simulation details.

-   Send a DELETE request to <http://localhost:9003/v1/simulations/1> to delete the current simulation. Verify with the previous GET command.

-   Send a **POST** method to <http://localhost:9003/v1/simulations> with the following characteristics:

    a.  Select **Headers** beneath the URL and add the following Key / Value pair:

        i.  Key: **Content-Type**

    b.  Value: **application/json**

    c.  Select **Body**, select **raw**, then **JSON (application/json)** as the content type. Paste the following to create a new simulation with our new models, plus existing elevator models:

    ```
    {
        "Enabled": true,
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
                    "Id": "elevator-01",
                    "Count": 3
                },
                {
                    "Id": "elevator-02",
                    "Count": 2
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
#### Exit criteria

-   You have successfully created a new simulation. Verify this with the GET request.

### Task 6: Run the device simulation agent locally

In this task, you will run the device simulation agent (SimulationAgent project) locally to run the simulation you just created. Because you created the new device elements locally, this is where you will run the simulation for the lab. To permanently have this run from the hosted Docker container, you would need to create a new Docker image, publish it to your own Docker Hub account, and either update the solution scripts to use the custom image and redeploy your solution to Azure, or ssh into the VM hosting the container and manually replace the image there. These steps are beyond the scope of the lab but can be found in the [developer reference guide](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide).

Run the simulator and let it continue running in the background.

#### Tasks to complete

-   Switch back to the **device-simulation** solution in Visual Studio. Right-click the **SimulationAgent** project, then select **Start new instance** under **Debug**, to run a new instance of the console app.

#### Exit criteria

-   You can see logging data in the console, showing the telemetry that is generated and sent to IoT Hub.

-   There are no errors in the console.

-   Continue running the console in the background for the remainder of the lab.

### Task 7: Create alerts and filters in the monitoring web app

The IoT Remote Monitoring web interface enables you to create filters that help group devices by type or other parameters. You can also create alerts that are fired when certain criteria are met, enabling you to see the alerts alongside your device data or on the map. In this task, you will create filters for your buses and traffic lights, then create an alert for traffic lights whose voltage exceed a predefined level.

#### Tasks to complete

-   Navigate back to the monitoring web app. If you don't remember the path or have closed the previous browser session, the naming convention is \[your solution name\].azurewebsites.net. You may need to refresh the browser window if it has been running for some time and is unresponsive.

-   One of the first things you may notice is that there are new telemetry data points listed above the graph. You should also see new devices showing up on the map over New York City. In the screenshot below, the new voltage telemetry option is selected, and data for the two new traffic lights appear beneath.\
    ![The Monitoring Web App dashboard displays with the previously described information.](images/Hands-onLabunguided-IoTforbusinessimages/media/image14.png "Monitoring Web App dashboard")

-   Create a new device group with the following parameters:

    -   Name: Buses

    -   Field: Properties.Reported.Type

    -   Operator: = Equals

    -   Value: Bus

    -   Type: Text

-   Create another device group with the following parameters in the form:

-   Name: Traffic Lights

-   Field: Properties.Reported.Type

-   Operator: = Equals

-   Value: Traffic Light

-   Type: Text

-   Create a new rule following values in the New rule form:

    -   Name: Voltage Too High

    -   Description: Traffic light voltage is higher than normal

    -   Source: select your Traffic Lights group

    -   Trigger:

    -   Field: voltage

    -   Operator: \>=

    -   Value: 74

    -   Severity level: Critical

    -   Rule status: Enabled

#### Exit criteria

-   You can view only buses or traffic lights using your new filters.

-   You eventually receive one or more occurrences of your Voltage Too High rule, and you can see which traffic light is affected by looking at the map.

### Task 8: Send jobs to IoT devices

In this task, you will send a job to one of the traffic light devices, using the DecreaseTiming job defined in the scripts folder of the device-simulation project.

#### Tasks to complete

-   Navigate back to the monitoring app's dashboard. Select the **timing** telemetry option. Observe the current timing for the traffic lights. One should consistently be 90 (seconds), and the other 65.\
    ![In the Telemetry section, timing (2) is selected.](images/Hands-onLabunguided-IoTforbusinessimages/media/image15.png "Telemetry section")

-   Select traffic light \#1 in devices, then schedule a **DecreaseTiming** job.

#### Exit criteria

-   Navigate back to the dashboard and view the **timing** telemetry once again. This time, you should notice that the traffic light timing for traffic light \#1 decreased from 90 seconds to 75.\
    ![In the Telemetry section, the decrease is circled on the graph.](images/Hands-onLabunguided-IoTforbusinessimages/media/image16.png "Telemetry section")

## Exercise 4: Create IoT Edge device and custom modules

**Duration:** 60 minutes

Azure IoT Edge devices can run on in locations where there is little to no internet connectivity, yet they allow you to run powerful modules locally, enabling you to apply your business logic in place. This is especially powerful when coupled with sensors that generate a lot of data, and you only want to send the most important data to the cloud.

In this scenario, IoT Edge devices will be installed on city buses. You will create a Stream Analytics module to filter the vehicle telemetry data from simulated sensors located within a custom module that you will also build and deploy. When the Stream Analytics module finds behavior consistent with aggressive driving or impending engine failure, the data will be sent to Azure via IoT Hub. The obvious benefit to this is that the massive amount of data can be analyzed locally, only the important data is sent over a slow, unreliable, or expensive cellular data connection.

### Help references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
| What is Azure IoT Edge?      | <https://docs.microsoft.com/en-us/azure/iot-edge/how-iot-edge-works>  |
| Understand the requirements and tools for developing IoT Edge modules  | <https://docs.microsoft.com/en-us/azure/iot-edge/module-development>  |
| Develop and deploy a C\# IoT Edge module to your simulated device   | <https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-csharp-module>  |
| Azure Stream Analytics on IoT Edge   | <https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-edge>  |


### Task 1: Add a new IoT Edge device

#### Tasks to complete

-   Add a new IoT Edge device in IoT Hub. Set the Device ID to **bus1**.

#### Exit criteria 

-   Copy the value for **Connection string--primary key** and save it. This will be used to configure the IoT Edge runtime.

### Task 2: Install and start the IoT Edge runtime

The IoT Edge runtime is deployed to all IoT Edge devices. It consists of two modules: The IoT Edge agent, which facilitates deployment and monitoring of modules on the device, and IoT Edge Hub, which manages communication between modules on the IoT Edge device, and between the device and IoT Hub. You will install and start the IoT Edge runtime on the Linux VM that you provisioned in Exercise 2.

#### Tasks to complete

-   SSH into your Linux VM you provisioned in Exercise 2.

-   Install Python Pip.

-   Install the IoT Edge control script.

-   Configure the runtime with your IoT Edge device connection string.

-   Start the runtime.

#### Exit criteria 

-   You can see the IoT Edge agent running as a module within Docker.

### Task 3: Create and deploy the custom C\# IoT Edge module for vehicle telemetry

In this task, you will use Visual Studio Code to complete the custom C\# IoT Edge module that simulates vehicle telemetry representing bus sensor data. Then you will create the Docker container, and register it in your Azure Container Registry instance so it can be deployed to the IoT Edge device.

#### Tasks to complete

-   Open Visual Studio Code on your development machine and open the Vehicle Telemetry Simulator folder where your lab files are.

-   Open Program.cs within the VehicleTelemetrySimulator\modules folder. Complete the code for TODO items 5, 6, and 7.

    -   Within the **Init** method:

        -   Set the local vin value to the VIN specified in the module's twin.

        -   Set the local borough value to the Borough specified in the module's twin.

    -   Within the **GenerateMessage** method:

        -   Use the ModuleClient instance (IoTHubModuleClient) to asynchronously send the event message, using the specified output name (outputName).

-   Execute a command in the VS Code integrated terminal to login to Docker with the username, password, and login server that you copied from your Azure Container Registry when you created it.

-   Build and Push the IoT Module.

-   Specify the following container image name: \<your container registry address\>/vehicle-telemetry-simulator

-   Add the Container Registry credentials to the Edge runtime. These credentials will give it access to pull the container.

#### Exit criteria 

-   You will see your container registry added to the Registries configuration for the Edge runtime.

-   You have copied the generated tag for the vehicle-telemetry-simulator Docker image. Should be similar to :0.0.1-amd64.

### Task 4: Create the Azure Stream Analytics IoT Edge module

In this task, you will create a Stream Analytics job that filters vehicle telemetry data generated by the custom C\# module, and outputs only the most important data to potentially two different outputs in IoT Hub.

#### Tasks to complete

-   Add a new container named **asa-container** with a public access level of Container to the provisioned Azure Storage account in your resource group.

-   Create a new Stream Analytics job and select the **Edge** hosting environment.

-   Create a new Input to your Stream Analytics job with the following configuration:

    -   Input alias: **VehicleTelemetry**

    -   Source: Edge Hub

    -   Event-serialization format: JSON

    -   Encoding: UTF-8

-   Create a new Output with the following configuration:

    -   Input alias: **Alert**

    -   Event-serialization format: JSON

    -   Encoding: UTF-8

-   Create a new output that will trigger the route you created in IoT Hub earlier that sends events filtered on the EngineAlert output, to the custom endpoint and on to the Service Bus Queue. Provide the following configuration:

    -   Input alias: **EngineAlert**

    -   Event-serialization format: JSON

    -   Encoding: UTF-8

-   Configure the Query with the following attributes:

    -   Create a step that averages the engine temperature and speed over a two second duration. Create another step that selects all telemetry data, including the average values from the previous step, and specifies the following anomalies as new fields:

        -   enginetempanomaly: When the average engine temperature is \>= 405 or \<= 15.

        -   oilanomaly: When the engine oil \<= 1.

        -   aggressivedriving: When the transmission gear position is in first, second, or third, and the brake pedal status is 1, the accelerator pedal position \>= 90, and the average speed is \>= 55.

    -   Have the query output all fields from the anomalies step into the Alert output where aggressivedriving = 1 or enginetempanomaly = 1.

    -   Have the query output all fields from the anomalies step where the enginetempanomaly = 1 and oilanomaly = 1.

#### Exit criteria 

-   Test your query using the **sample-vehicle-telemetry.json** file extracted to the IoTLab folder from the starter solution zip file you downloaded. This file contains \~5000 JSON records of simulated vehicle telemetry.

-   The Alert output should have around 2,231 rows, and the EngineAlert output 48.

    ![In the Results section, enginealert and 48 rows are both selected.](images/Hands-onLabunguided-IoTforbusinessimages/media/image17.png "Results section")

### Task 5: Deploy the custom modules to IoT Edge device

In this task, you will deploy the vehicle telemetry module and Stream Analytics module to the IoT Edge device. Both will be deployed simultaneously so you can register the module routes to send vehicle telemetry data to the Stream Analytics module, then send the filtered data upstream to IoT Hub as needed.

#### Tasks to complete

-   Add a new IoT Edge Module to your IoT Edge device, using the following configuration values:

    -   Name: VehicleTelemetry

    -   Image URI: The image URI path you specified when you created the Docker image and registered it in Azure Container Registry. Should be in the form of \<your container registry address\>/vehicle-telemetry-simulator:latest

    -   Restart Policy: always

    -   Desired Status: running

    -   Module twin's desired properties: Enable and enter the following into the configuration area:


    ```
    {
        "properties.desired": {
            "VIN":"Y3J9PV9TN36A4DUB9",
            "Borough":"Manhattan"
        }
    }
    ```

-   Add your Azure Stream Analytics job as a new module. Make sure you use the **asa-container** you created in the storage account.

-   Copy the following code to Routes. Replace *{moduleName}* with the Stream Analytics module name:

    ```
    {
        "routes": {
            "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream",
            "telemetryToAsa": "FROM /messages/modules/VehicleTelemetry/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/VehicleTelemetry\")"
        }
    }

    ```
#### Exit criteria 

-   You should see the two new modules running, along with the IoT Edge agent module and the IoT Edge hub.

    ![On the Deployed Modules tab, under Name, iot-lab-edge and VehicleTelemetry are called out.](images/Hands-onLabunguided-IoTforbusinessimages/media/image18.png "Device Details page Deployed Modules tab")

-   View the Docker containers running on your Linux VM where the IoT Edge device is running. You should see four containers running.

-   View the Stream Analytics module logs to see the telemetry it is reading, as well as any outputs it generates based on anomalies. You should see a large degree more vehicle telemetry feeding into the Stream Analytics module than what it sends out. This, of course, is by design. Replace {moduleName} with the Stream Analytics module name.

    docker logs -f {moduleName}

-   Notice the log output as shown below. There are many "Received message Name: \[VehicleTelemetry\]" events, and one output generated (highlighted). The output name is **alert**, matching one of the two outputs we created in the Stream Analytics module. The message content is sent to IoT Hub, including the additional fields added by the Stream Analytics query. In this case, the telemetry data is flagged as aggressive driving (aggressivedriving: 1).

> ![In the Output window, results from the Stream Analytics module logs display.](images/Hands-onLabunguided-IoTforbusinessimages/media/image19.png "Output window")

-   Leave the IoT Edge device running for the remainder of the lab.

##  Exercise 5: Run a console app to view critical engine alerts from the Service Bus Queue

**Duration:** 10 minutes

As you remember, you created an Azure Service Bus Queue to ingest messages flagged as critical engine alerts. This queue is fed messages from a custom endpoint in IoT Hub when routing rules match telemetry flowing from the IoT Edge device whose Stream Analytics module output is equal to EngineAlert. In this exercise, you will execute a simple console app that reads the Service Bus queue one-at-a-time so you can easily view the contents of the alerts.

### Help references

  --------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------
  Use the Service Bus .NET SDK to receive messages from a queue:   <https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-dotnet-get-started-with-queues#4-receive-messages-from-the-queue>.
  --------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------

### Task 1: Make sure the Service Bus Queue is receiving messages

#### Tasks to complete

-   View the alert-q Service Bus queue to make sure you have at least one active message.

#### Exit criteria

-   The queue should have at least one active message.

-   You copied and saved the Service Bus Primary Connection String from the RootManagerSharedAccessKey policy.

### Task 2: Configure and execute the ReadEngineAlerts solution in Visual Studio 2017

#### Tasks to complete

-   Open **ReadEngineAlerts\\ReadEngineAlerts.sln** in your lab-files folder containing the lab solution files.

-   Set the **connectionString** variable in Program.cs with the Service Bus connection string you copied.

-   Run the console app.

#### Exit criteria

-   You should start to see critical alerts flowing in to the console window.

    ![Critical alerts display with varying colors in the Console window.](images/Hands-onLabunguided-IoTforbusinessimages/media/image20.png "Console window")

-   Notice that all values have a 1 in both the enginetempanomaly and oilanomaly properties. This is due to the filter you defined within the Stream Analytics IoT Edge module.

-   You may close the console when you are finished reviewing the data.

## Exercise 6: Create Azure Function App to ingest critical engine alerts and store them in Cosmos DB

**Duration:** 45 minutes

The console app is a fast and easy way to view the critical engine alerts flowing through the Service Bus Queue, but it isn't very practical for establishing any sort of business or processing workflow. Azure Functions makes it easy to ingest messages from service Bus, because it provides a Service Bus trigger that executes the function and passes the message object as soon as it is added to the queue. The automated scalability of Azure Functions means that it will add more resources as needed to keep up with demand during peak loads, then scales the resources back down during quieter periods. Azure Functions also provides a Cosmos DB output that makes it very simple to write data to Cosmos DB, which is another massively scalable service. Since other data is already being stored in Cosmos DB, it is a natural choice for these alerts.

### Help references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
| Azure Service Bus bindings for Azure Functions   | <https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus>   |
| Azure Cosmos DB bindings for Azure Functions   |  <https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-cosmosdb> |


### Task 1: Create new Cosmos DB collection

The function will output to two different Cosmos DB collections. The first collection is the messages collection that stores the IoT device telemetry flowing through the IoT Remote Monitoring solution. This will allow you to view bus engine data that meets the alert criteria, within the monitoring web application. The second collection will be the one you create in this task for storing the entire contents of the incoming alert data.

There are currently two databases and three collections in Cosmos DB:

-   Database: **pcs-storage**

-   Collection: **pcs-storage**

    -   Description: contains configuration data for the IoT Remote Monitoring solution. This is where the device groups, such as the Bus and Traffic Light filters you created earlier are stored. You will also find the defined rules and the simulation you created through the REST API.

-   Database: **pcs-iothub-stream**

    -   Collection: **alarms**

        -   Description: As the microservice that executes the EventProcessor to read incoming IoT Hub messages processes the data, it initiates a database write to both collections in this database. Alarms contains the alarm data for those messages matching the criteria of one or more rules.

    -   Collection: **messages**

        -   Description: This is the collection that holds all IoT Hub messages that the EventProcessor microservice can parse. This does not include messages sent by the IoT Edge device, because the messages lack the additional metadata properties used to apply the data schema. Thus, those messages are ignored. Our Azure function will write the critical engine alert data to this collection in a format that is compatible with the web interface.

#### Tasks to complete

-   Create a new collection in the **pcs-iothub-stream** database, named **critical-alerts**. This should have a fixed storage capacity and a throughput of 1000.

#### Exit criteria 

-   Your Cosmos DB has a new collection for storing critical alerts.

### Task 2: Create a new Function App

#### Tasks to complete

-   Create a new Function App with a Consumption Plan.

#### Exit criteria 

-   You have a new Function App added to your lab's resource group.

### Task 3: Create a new Function

#### Tasks to complete

-   Create a new Function using the **JavaScript** language and the **Service Bus Queue trigger template**.

-   Name it **BusEngineAlert**.

-   Set the access rights to Manage, and point it to the **alert-q** queue.

#### Exit criteria 

-   Your Function App has a new JavaScript Function that is triggered by your Service Bus Queue.

### Task 4: Add an Azure Cosmos DB output to the messages collection

#### Tasks to complete

-   Provide the following values for the Azure Cosmos DB output:

    -   **Document parameter name**: outputDocument

    -   **Database name**: pcs-iothub-stream

    -   **Collection Name**: messages

#### Exit criteria 

-   Your new Function has an Azure Cosmos DB output that stores messages in the *messages* collection.

### Task 5: Add an Azure Cosmos DB output to the critical-alerts collection

#### Tasks to complete

-   Provide the following values for the Azure Cosmos DB output:

    -   **Document parameter name**: alarmDocument

    -   **Database name**: pcs-iothub-stream

    -   **Collection Name**: critical-alerts

#### Exit criteria 

-   Your new Function has an Azure Cosmos DB output that stores critical alerts in the *critical-alerts* collection.

### Task 6: Modify the function code

#### Tasks to complete

-   The function code should have the following characteristics:

    -   Extract the IoT Edge device id from the context bindingData properties collection.

    -   Create a Unix Epoch value from the context bindingData enqueuedTimeUtc value. This will create a timestamp for when the message was received.

    -   Create a Unix Epoch value for the current date/time, which is used for the message created timestamp.

    -   Create a new JSON document with the following schema to save data to the **messages** Azure Cosmos DB collection:

    
    ```
    {
        "id": deviceId + ";" + receivedEpoch,
        "doc.schema": "d2cmessage",
        "doc.schemaVersion": 1,
        "device.id": deviceId,
        "device.msg.schema": "bus-edge;v1",
        "device.msg.created": createdEpoch,
        "device.msg.received": receivedEpoch,
        "data.vin": "",
        "data.enginetemperature": 0.0,
        "data.averageenginetemperature": 0.0,
        "data.speed": 0.0,
        "data.averagespeed": 0.0,
        "data.engineoil": 0,
        "data.enginetempanomaly": 0,
        "data.oilanomaly": 0,
        "data.aggressivedriving": 0
    }

    ```

-   Create a new JSON document containing all values from the incoming alert message, saving it to the **critical-alerts** collection.

#### Exit criteria

-   Select **Run** to view the output logs of the function. You should see successful function completed events.\
    ![Output logs display, with two success messages highlighted.](images/Hands-onLabunguided-IoTforbusinessimages/media/image21.png "Output logs")

### Task 7: View data stored by the function in Azure Cosmos DB

Apply a query filter on the messages collection to view specially-formatted critical engine alert messages formatted for viewing in the monitoring website. Then view the full message data saved to the critical-alerts collection.

#### Tasks to complete

-   Execute the following query on the **messages** collection in Azure Cosmos DB:

    ```
    SELECT * FROM c where c["device.msg.schema"] = 'bus-edge;v1'

    ```
    
    ![On the Query 1 tab, the Results 1 - 87 message is called out, and results display below.](images/Hands-onLabunguided-IoTforbusinessimages/media/image22.png "Query 1 tab")

-   Explore the documents within the **critical-alerts** collection to view the full message data for the critical alerts.

#### Exit criteria

-   One or more documents exist in both collections.

### Task 8: View the critical engine alerts in the IoT Remote Monitoring web interface

Now that the critical engine alert data is being added to the messages collection in the proper format, go to the monitoring web UI and view it.

#### Tasks to complete

-   Navigate back to the monitoring web app. If you don't remember the path or have closed the previous browser session, the naming convention is \[your solution name\].azurewebsites.net. You may need to refresh the browser window if it has been running for some time and is unresponsive.

-   You should see additional telemetry options above the graph at the bottom of the page, indicating availability of data from the new fields that are part of the critical engine alerts. Select **averageenginetemperature**.

    ![The Monitoring Web App dashboard displays a Telemetry graph of averageenginetemperature \[1\] for bus1.](images/Hands-onLabunguided-IoTforbusinessimages/media/image23.png "Monitoring Web App dashboard")

-   The temperature range never drops below the minimum temperature threshold set by the Stream Analytics filter Also, you will notice that the IoT Edge device name is listed underneath the chart.

-   Select enginetemperature from the telemetry filter above the chart. There you will see some points where the temperature drops well below the minimum threshold, but all in all, the values are very high. This is to be expected, because the anomaly query in Stream Analytics accounts for a sliding window average of 2 seconds.

    ![The Monitoring Web App dashboard displays a Telemetry graph of enginetemperature \[1\] for bus1.](images/Hands-onLabunguided-IoTforbusinessimages/media/image24.png "Monitoring Web App dashboard")

#### Exit criteria

-   You can see telemetry data from your IoT Edge device.

### Task 9: Add tag to Edge's device twin and create new filter from it

In this task, you will create the following tag that will be saved to the device twin: Name: IsEdgeDevice, Value: Y. You will then use this tag as the basis for a new device filter in the web UI for displaying Edge devices.

#### Tasks to complete

-   Select your Bus1 IoT Edge device in the monitoring app's Devices list, then create a tag using the following configuration for the new tag:

    -   **Job Name:** edgetag

    -   Select **+ Add** tag, then enter the following values:

        -   **Name**: IsEdgeDevice

        -   **Value:** Y

-   Open the IoT Edge device's device twin in IoT Hub. You should see the new tag in the device twin. If not, try clicking the **refresh** button.

    ![In the Device Twin blade, the new tag is called out. ](images/Hands-onLabunguided-IoTforbusinessimages/media/image25.png "Device Twin blade")

-   Create a new filter in the monitoring app that targets Edge devices. Provide the following configuration for the new filter:

    -   **Filter Name**: Edge Devices

    -   **Field**: tags.IsEdgeDevice

    -   **Operato**r: = Equals

    -   **Value**: Y

    -   **Type**: String

#### Exit criteria

-   The IoT Edge device's device twin is successfully updated.

-   Your Edge Devices filter functions so that only one device is reported and displayed.

    ![The Monitoring Web App dashboard displays information for only one Edge Device.](images/Hands-onLabunguided-IoTforbusinessimages/media/image26.png "Monitoring Web App dashboard")

## Exercise 7: View all data in Azure Time Series Insights

**Duration**: 15 minutes

Now that the critical engine alert data is being added to the messages collection in the proper format, go to the monitoring web UI and view alerts.

### Help references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
| What is Azure Time Series Insights?  | <https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-overview>   |
| Azure Time Series Insights explorer  | <https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-explorer>  |


### Task 1: Add your account as a Contributor to the Data Access Policies

Before you can access the Time Series Insights environment and make changes, you need to make sure your account is added as a Contributor to the Data Access Policies. This is where you add additional users who can access this data.

#### Tasks to complete

-   Open the Data Access Policies blade in your Time Series Insights instance. Make sure your account is listed with the Reader and Contributor roles.

#### Exit criteria

-   You can successfully access your Time Series Insights environment.

### Task 2: Go to the Time Series Insights environment and use the data explorer

#### Tasks to complete

-   Add a new query in the Terms Editor Panel. Select **voltage** under Measure, then **serial\_number** under Split By.

-   You should now see the two traffic lights listed by serial number, on the same chart as the total event count. You can hover over a serial number to the left of the chart to highlight its place on the chart.

    ![This time, 3 series of 3 total display in chart format in the Time Series Insights Data Explorer.](images/Hands-onLabunguided-IoTforbusinessimages/media/image27.png "Time Series Insights Data Explorer")

-   Modify the terms you added to use Step Interpolation and show Min/Max.

-   The chart will update to reflect those changes. Notice how Show Min/Max creates a shadow behind the interpolated line to show the minimum and maximum voltage values. This is because the displayed line is the average voltage value.

    Time Series Insights Data Explorer ![The described multi-dimensional chart displays in the Time Series Insights Data Explorer.](images/Hands-onLabunguided-IoTforbusinessimages/media/image28.png "Time Series Insights Data Explorer")

-   Explore events for one of the lines on the chart.

-   You will see all the raw event data for the events displayed within the visible time range for the selected terms. You may select which columns you want to view and export the data if desired.\
    ![The Events tab displays. Select all columns is selected, and raw data displays.](images/Hands-onLabunguided-IoTforbusinessimages/media/image29.png "Events tab")

#### Exit criteria

-   You were able to view all of the data based on the defined queries.

### Task 3: View the simulated and IoT Edge bus data side-by-side

Because all telemetry data that flows through IoT Hub is captured in Time Series Insights, it is possible to view this information in one place, creating a data context that's based on time of occurrence and any additional data points you need. In this case, we will overlay the simulated bus GPS device data from the IoT Remote Monitoring simulator, with the vehicle telemetry data generated and filtered by the bus IoT Edge device.

#### Tasks to complete

-   Delete the traffic light voltage query and add a new query set in the Terms Editor Panel. Select **averageenginetemperature** as the Measure, and Split By **none**. Update the Where clause with the following:

    vin = \'Y3J9PV9TN36A4DUB9\'

-   Add a new query set. Select **speed** as the measure, and **Split By** iothub-connection-device-id. Update the Where clause with the following:

    vin = \'Y3J9PV9TN36A4DUB9\' AND \[iothub-connection-module-id\] = NULL

-   The where clause ensures that the simulated bus GPS device data matches up with the bus IoT Edge device by setting the VIN for both to the same value. Since both event sources contain a "speed" field, we want to rely on the reported speed from the GPS device, as it is updated more regularly. To do this, the where clause returns only those items whose "iothub-connection-module-id" is null. Only Edge devices will contain an Id in this field.

#### Exit criteria

-   The chart shows both the average engine temperature and the reported bus speed from the GPS device stacked one on top of the other.

    ![This time, 3 series of 3 total graphs display in the Time Series Insights Data Explorer.](images/Hands-onLabunguided-IoTforbusinessimages/media/image30.png "Time Series Insights Data Explorer")

-   To overlay the data, hide the overall event Count in the Terms Editor Panel, use step interpolation on the Edge and simulated bus queries to make it easier to see the overlaid values, then deselect the stack terms option on the chart. This view may make it easier to spot correlations between different data sets, such as this one.

    ![2 series of 2 total graphs display in the Time Series Insights Data Explorer, and the overlay icon is selected.](images/Hands-onLabunguided-IoTforbusinessimages/media/image31.png "Time Series Insights Data Explorer")

### Task 4: Use Perspective View to create a simultaneous view of up to four unique queries

#### Tasks to complete

-   Open Perspective View, then clone the chart to the empty space to the right.

-   Modify the cloned chart.

-   Remove the speed terms, then modify the Edge device terms (those based on the average engine temperature) by selecting **transmission\_gear\_position** under Split By. This should now be the only visible term.

-   Select **Heatmap** for the chart type. It is easy to quickly spot anomalies in this view, where there are random hot and cold spots. This data is split by the transmission gear position so you can try and correlate engine temperatures with transmission gears.

    ![A heatmap displays in the Time Series Insights Data Explorer.](images/Hands-onLabunguided-IoTforbusinessimages/media/image32.png "Time Series Insights Data Explorer")

-   Go back to the Perspective View to see these two charts side-by-side.

    ![In the Time Series Insights Data Explorer Perspective view, a chart displays on the left, and a heat map displays on the right.](images/Hands-onLabunguided-IoTforbusinessimages/media/image33.png "Time Series Insights Data Explorer")

-   Select the **Save** icon to share the Perspective, give it a name, and save it.

#### Exit criteria

-   You have a perspective view with, at minimum, two charts. One chart overlays the average engine temperature and bus speed values, and the other is a heat map showing average engine temperatures for the different transmission gears.

## After the hands-on lab 

**Duration:** 10 minutes

In this exercise, attendees will deprovision any Azure resources that were created in support of the lab.

### Task 1: Delete the Resource Group in which you placed your Azure resources.

-   From the Portal, navigate to the blade of your Resource Group and click Delete in the command bar at the top.

-   Confirm the deletion by re-typing the resource group name and clicking Delete.

You should follow all steps provided *after* attending the hands-on lab.

