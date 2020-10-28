# IoT and the Smart City team challenge

Fabrikam City council has conducted a six-month study of new and emerging technologies that can improve the lives of its citizens. Being the largest city in the US, the challenges most cities face are compounded by scale. Many of these challenges revolve around city traffic and public transportation.

At the conclusion of their study, the city council realized that the Internet of Things (IoT) is widely available and is becoming more integrated into our daily lives. Fabrikam City can capitalize on the wide availability and affordability of IoT devices. This means physical things like traffic lights and vehicles will be able to collect and share data by connecting to the Internet. Through analytics, cities can turn this data into intelligent information that will change the way the world works.

## Contents

- [IoT and the Smart City team challenge](#iot-and-the-smart-city-team-challenge)
  - [Contents](#contents)
  - [Abstract](#abstract)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Before the team challenge](#before-the-team-challenge)
  - [Team challenge](#team-challenge)

## Abstract

The IoT and the Smart City team challenge is an exercise that will challenge you to implement an end-to-end scenario using a supplied sample that is based on IoT Hub, IoT Edge devices, Cosmos DB, Time Series Insights, Service Bus, and related Azure services. The team challenge can be implemented on your own, but it is highly recommended to pair up with other members at the lab to model a real-world experience and to allow each member to share their expertise for the overall solution.

## Solution architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully so you understand the whole of the solution as you are working on the various components.

![The Solution Architecture diagram is described in the text following the diagram.](images/Hands-onlabstep-by-step-IoTandtheSmartCityimages/media/image2.png 'Solution Architecture diagram')

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

## Before the team challenge

Before attending the team challenge workshop, you should set up your environment for use in the rest of the challenge.

You should follow all the steps provided in the [Before the team challenge](before-the-team-challenge-iot-smart-city.md) section to prepare your environment before attending the team challenge. Failure to complete the `Before the team challenge` setup may result in an inability to complete the challenge within the time allowed.

## Team challenge

- [Challenge guide](team-challenge-iot-smart-city.md)
  - Provides detailed challenge instructions.
