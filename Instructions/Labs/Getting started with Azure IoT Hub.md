# Getting Started with Azure IoT Services

## Lab Scenario

You are an Azure IoT Developer working for Contoso, a company that crafts and distributes gourmet cheeses.

You have been tasked with exploring Azure and the Azure IoT services that you will using to develop Contoso's IoT solution. You have already become familiar with the Azure portal and created a resource group for your project. Now you need to begin investigating the Azure IoT services.

## In This Lab

In this lab, you will create and examine an Azure IoT Hub and an IoT Hub Device Provisioning Service. The lab includes the following exercises:

* Explore Globally Unique Resource Naming Requirements
* Create an IoT Hub using the Azure portal
* Examine features of the IoT Hub service
* Create a Device Provisioning Service and link it to your IoT Hub
* Examine features of the Device Provisioning Service

## Lab Instructions

### Exercise 1: Explore Globally Unique Resource Naming Requirements

In labs 2-20 of this course, you will be creating Azure resources that are used to develop your IoT solution. To ensure consistency across the labs and to help in tidying up resources when you are finished with them, suggested resource names will be provided within the lab instructions. As much as possible, the suggested resource names will follow the naming guidelines recommended here: [Recommended naming and tagging conventions](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging). However, many of the resources that you will create during this course expose services that can be consumed across the web, which means that they must have globally unique names. To ensure that these resources satisfy the globally unique requirement, you will be adding a unique identifier to the end of the resource names when needed.

In this exercise, you will create your unique ID and review some examples that help to illustrate how you will use your unique ID during labs 2-20 of this course.

#### Task 1: Create Your Unique ID

1. Construct your unique ID by using your lower-case initials and the current date in the following pattern:

    ```text
    YourInitialsYYMMDD
    ```

    The first part of your unique ID will be your initials in lower-case. The second part will be the last two digits of the current year, the current numeric month, and the current numeric day. Here are some examples:

    ```text
    gwb200123
    bho200504
    cah201216
    dm200911
    ```

    Within the lab instructions, you will see `{your-id}` listed as part of the suggested resource name whenever you need to enter your unique ID. The `{your-id}` portion of the suggested resource name is a placeholder. You will replace the entire placeholder string (including the `{}`) with your unique value.

1. Make a note of your unique ID now and then **use the same value through the entire course**.

    > **Note**: Don't change the date portion of your unique ID each day. Use the same unique ID each day of the course.

#### Task 2: Review How and When to Apply Your Unique ID

Many of the resources that you create during the labs in this course will have publicly-addressable (although secured) endpoints and therefore must be globally unique. Examples of resources that require globally unique names include IoT Hubs, Device Provisioning Services, and Azure Storage Accounts.

As noted above, when you create these types of resources, you will be provided with a resource name that follows suggested guidelines and you will be instructed to include your unique ID as part of the resource name. To help clarify when you need to enter your unique ID, the suggested resource name will include a placeholder value for your unique ID. You will be instructed to replace the placeholder value, `{your-id}`, with your unique ID.

1. Review the following resource naming examples:

    If your Unique ID is: **cah191216**

    | Resource Type | Name Template | Example |
    | :--- | :--- | :--- |
    | IoT Hub | iot-az220-training-{your-id} | iot-az220-training-cah191216 |
    | Device Provisioning Service | dps-az220-training-{your-id} | dps-az220-training-cah191216 |
    | Azure Storage Account <br/>(name must be lower-case and no dashes) | az220storage{your-id} | az220storagecah191216 |

1. Review the following example for applying your unique ID within a Bash script:

    In some of the labs later in this course, you will be instructed to apply your unique ID value within a Bash script. The Bash script file, which is provided for you, might include code that is similar to the following:

    ```bash
    #!/bin/bash

    YourID="{your-id}"
    RGName="rg-az220"
    IoTHubName="iot-az220-training-$YourID"
    ```

    In the code above, if the value of your unique ID is `cah191216`, then the line containing `YourID="{your-id}"` should be updated to `YourID="cah191216"`.

    > **Note**: Notice that you do not change the `$YourID` value on the final code line. If it isn't `{your-id}` then don't replace it.

1. Review the following example for applying your unique ID within C# code:

    In some of the labs later in this course, you will be instructed to apply your unique ID value within C# source files. The C# source code, which will be provided to you, might include a code section that looks similar to the following:

    ```csharp
    private string _yourId = "{your-id}";
    private string _rgName = "rg-az220";
    private string _iotHubName = $"iot-az220-training-{_yourId}";
    ```

    In the code above, if the value of your unique ID is `cah191216`, then the line containing `private string _yourId = "{your-id}";` should be updated to `private string _yourId = "cah191216";`

    > **Note**: Notice that you do not change the `_yourId` value on the final code line. Once again, if it isn't `{your-id}` then don't replace it.

1. Notice that not all resource names require you to apply your unique ID.

    As you may have already considered, the Resource Group that you created in the previous lab did not include your unique ID value.

    Some resources, like the Resource Group, must have a unique name within your subscription, but the name does not need to be globally unique. Therefore, each student taking this course can use the resource group name: **rg-az220**. Of course this is only true if each student uses their own subscription, but that should be the case.

1. Apply an additional `01` or `02` if it turns out that your unique ID isn't so unique.

    It could happen that two or more people with the same initials start the course on the same day. You may not know, unless the person is sitting next to you, until you create your IoT Hub in the next exercise. Azure will let you know if the suggested resource name, including your unique ID, isn't globally unique. In that case you will be instructed to update your unique ID by appending an additional `##` value. For example, if the value of your unique ID is `cah191216`, your updated unique ID value could become: 

    ```text
    cah20121600
    cah20121601
    cah20121602
    ...
    cah20121699
    ```

    If you do have to update your unique ID, try to use it consistently.

### Exercise 2: Examine the IoT Hub Service

As you have already learned, IoT Hub is a managed service, hosted in the cloud, that acts as a central message hub for bi-directional communication between your Azure IoT services and your connected devices.

IoT Hub's capabilities help you build scalable, full-featured IoT solutions such as managing industrial equipment used in manufacturing, tracking valuable assets in healthcare, monitoring office building usage, and many more scenarios. IoT Hub monitoring helps you maintain the health of your solution by tracking events such as device creation, device failures, and device connections.

In this exercise, you will examine some of the features that IoT Hub provides. 

#### Task 1: Explore the IoT Hub Overview information

1. If necessary, log in to [portal.azure.com](https://portal.azure.com) using your Azure account credentials.

    If you have more than one Azure account, be sure that you are logged in with the account that is tied to the subscription that you will be using for this course.

1. Verify that your AZ-220 dashboard is being displayed.

1. On the **rg-az220** resource group tile, click **iot-az220-training-{your-id}**

    When you first open your IoT Hub blade, the **Overview** information will be displayed. As you can see, the area at the top of this blade provides some essential information about your IoT Hub service, such as datacenter location and subscription. But this blade also includes tiles that provide information about how you are using your hub and recent activities. Let's take a look at these tiles before exploring further.

1. At the bottom-left of your IoT Hub blade, notice the **IoT Hub Usage** tile.

    > **Note**:  The tiles positions are based upon the width of the browser window, so the layout may be a little different than described.

    This tile provides a quick overview of what is connected to your hub and message count. As you add devices and start sending messages, this tile will provide nice "at-a-glance" information.

1. To the right of the **IoT Hub Usage** tile, notice the **Device twin operations** tile and the **Device to cloud messages** tile.

    The **Device to cloud messages** tile provides a quick view of the incoming messages from your devices over time. You will be registering a device and sending messages to your hub during a lab in the next module, so you will begin to see information on these tiles pretty soon.

    The **Device twin operations** tile can help you to keep track of device twin access. You will be learning about device twins and device twin operations during the modules of this course that cover device configuration, device provisioning, and device management. For now all you need to know is that each device that you register with your IoT Hub will have a device twin that you can use when you need to manage the device.

#### Task 2: View features of IoT Hub using the left-side menu

1. On the IoT Hub blade, take a minute to scan the left-side menu options.

    As you would expect, these menu options are used to open panes that provide access to properties and features of your IoT Hub. For example, some panes provides access to devices that are connected to your hub.

1. On the left-side menu, under **Explorers**, click **IoT devices**

    This pane can be used to add, modify, and delete devices registered to your hub. You will get pretty familiar with this pane by the end of this course.

1. On the left-side menu, near the top, click **Activity log**

    As the name implies, this pane gives you access to a log that can be used to review activities and diagnose issues. You can also define queries that help with routine tasks. Very handy.

1. On the left-side menu, under **Settings**, click **Built-in endpoints**

    IoT Hub exposes "endpoints" that enable external connections. Essentially, an endpoint is anything connected to or communicating with your IoT Hub. You should see that your hub already has two endpoints defined:

    * _Events_
    * _Cloud to device messaging_

1. On the left-side menu, under **Messaging**, click **Message routing**

    The IoT Hub message routing feature enables you to route incoming device-to-cloud messages to service endpoints such as Azure Storage containers, Event Hubs, and Service Bus queues. You can also create routing rules to perform query-based routes.

1. At the top of the **Message routing** pane, click **Custom endpoints**.

    Custom endpoints (such as Service Bus queue and Storage) are often used within an IoT implementation.

1. Take a minute to scan through the menu options under **Settings**

    > **Note**:  This lab exercise is only intended to be an introduction to the IoT Hub service and get you more comfortable with the UI, so don't worry if you feel a bit overwhelmed at this point. You will be configuring and managing your IoT Hub, devices, and their communications as this course continues.

### Exercise 3: Examine the Device Provisioning Service

The IoT Hub Device Provisioning Service is a helper service for IoT Hub that enables zero-touch, just-in-time provisioning to the right IoT hub without requiring human intervention, enabling customers to provision millions of devices in a secure and scalable manner.

#### Task 1: Explore the Device Provisioning Service Overview information

1. If necessary, log in to [portal.azure.com](https://portal.azure.com) using your Azure account credentials.

    If you have more than one Azure account, be sure that you are logged in with the account that is tied to the subscription that you will be using for this course.

1. Verify that your AZ-220 dashboard is being displayed.

1. On the **rg-az220** resource group tile, click **dps-az220-training-{your-id}**

    When you first open your Device Provisioning Service instance, it will display the Overview information. As you can see, the area at the top of the blade provides some essential information about your DPS instance, such as status, datacenter location and subscription. This blade also provides the _Quick Links_ section, which provide access to:

    * [Azure IoT Hub Device Provisioning Service Documentation](https://docs.microsoft.com/en-us/azure/iot-dps/)
    * [Learn more about IoT Hub Device Provisioning Service](https://docs.microsoft.com/en-us/azure/iot-dps/about-iot-dps)
    * [Device Provisioning concepts](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-service)
    * [Pricing and scale details](https://azure.microsoft.com/en-us/pricing/details/iot-hub/)

    When time permits, you can come back and explore these links.

#### Task 2: View features of Device Provisioning Service using the navigation menu

1. Take a minute to scan the left-side menu options.

    As you might expect, these options open panes that provide access to activity logs, properties and feature of the DPS instance.

1. On the left-side menu, near the top, click **Activity log**

    As the name implies, this pane gives you access to a log that can be used to review activities and diagnose issues. You can also define queries that help with routine tasks. Very handy.

1. On the left-side menu, under **Settings**, click **Quick Start**.

    This pane lists the steps to start using the Iot Hub Device Provisioning Service, links to documentation and shortcuts to other blades for configuring DPS.

1. On the left-side menu, under **Settings**, click **Shared access policies**.

    This pane provides management of access policies, lists the existing policies and the associated permissions.

1. On the left-side menu, under **Settings**, click **Linked IoT hubs**.

    Here you can see the linked IoT Hub from earlier. The Device Provisioning Service can only provision devices to IoT hubs that have been linked to it. Linking an IoT hub to an instance of the Device Provisioning service gives the service read/write permissions to the IoT hub's device registry; with the link, a Device Provisioning service can register a device ID and set the initial configuration in the device twin. Linked IoT hubs may be in any Azure region. You may link hubs in other subscriptions to your provisioning service.

1. On the left-side menu, under **Settings**, click **Certificates**.

    Here you can manage the X.509 certificates that can be used to secure your Azure IoT hub using the X.509 Certificate Authentication. You will investigate X.509 certificates in a later lab.

1. On the left-side menu, under **Settings**, click **Manage enrollments**.

    Here you can manage the enrollment groups and individual enrollments.

    Enrollment groups can be used for a large number of devices that share a desired initial configuration, or for devices all going to the same tenant. An enrollment group is a group of devices that share a specific attestation mechanism. Enrollment groups support both X.509 as well as symmetric. All devices in the X.509 enrollment group present X.509 certificates that have been signed by the same root or intermediate Certificate Authority (CA). Each device in the symmetric key enrollment group present SAS tokens derived from the group symmetric key. The enrollment group name and certificate name must be alphanumeric, lowercase, and may contain hyphens.

    An individual enrollment is an entry for a single device that may register. Individual enrollments may use either X.509 leaf certificates or SAS tokens (from a physical or virtual TPM) as attestation mechanisms. The registration ID in an individual enrollment is alphanumeric, lowercase, and may contain hyphens. Individual enrollments may have the desired IoT hub device ID specified.

1. Take a minute to review some of the other menu options under **Settings**

   > **Note**:  This lab exercise is only intended to be an introduction to the IoT Hub Device Provisioning Service and get you more comfortable with the UI, so don't worry if you feel a bit overwhelmed at this point. You will be covering DPS in much more detail as the course continues.
