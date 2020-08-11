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

### Exercise 3: Create and Test a Simulated Device (C#)

#### Task 1: Get the Device Connection String

In order for a device to connect to an IoT Hub, it needs to establish a connection. In this lab, you will use a connection string to connect your device directly to the IoT Hub (this for of authentication is often referred to as symmetric key authentication). When using Symmetric key authentication, there are two connection strings available - one that utilizes the Primary key, the other that uses the Secondary key. As noted above, the Primary and Secondary keys are only generated once the device record is saved. Therefore, to obtain one of the connection strings, you must first save the record (as you did in the task above) and then re-open the device record (which is what you are about to do).

1. On the **IoT devices** pane of your IoT Hub, under **DEVICE ID**, click **sensor-th-0001**.

1. Take a minute to review the contents of the **sensor-th-0001** device detail blade.

    In addition to the device properties, notice that the device detail blade provides access to a number of device related functions (such as Direct Method and Device Twin) along the top of the blade.

1. Notice that the key and connection string values are now populated.

    The values are obfuscated by default, but you can click the "eye" icon on the right of each field to toggle between showing and hiding the values.

1. To the right of the **Primary Connection String** field, click **Copy**.

    You can hover your mouse pointer over the button icons to display their names; the Copy button is on the far right.

    > **Note**: You will need to use the Primary Connection String value later in the lab, so you may want to save it to an accessible location (perhaps by pasting the value into a text editor such as NotePad). 

    The connection string will be in the following format:

    ```text
    HostName={IoTHubName}.azure-devices.net;DeviceId=sensor-th-0001;SharedAccessKey={SharedAccessKey}
    ```
#### Task 2: Create the initial project

1. Open a new command-line / terminal window.

    For example, you can use the Windows **Command Prompt** command-line application.

1. Navigate to the folder location where you want to create your simulated device application.

    The root folder location is not critical, but something easy to find with a short folder path amy be helpful.

1. At the command prompt, to create a directory named "CaveDevice" and change the current directory to that directory, enter the following commands:

   ```bash
   mkdir CaveDevice
   cd CaveDevice
   ```

1. To create a new .NET console application, enter the following command:

    ```bash
    dotnet new console
    ```

    This command creates a **Program.cs** file in your folder, along with a project file.

1. To install the Azure IoT Device SDK and code libraries required for your simulated device app, enter the following commands:

    ```bash
    dotnet add package Microsoft.Azure.Devices.Client
    ```

    > **Note**: The **Microsoft.Azure.Devices.Client** package contains the Azure IoT Device SDK for .NET and has the **Newtonsoft.Json** package as a dependency. The **Newtonsoft.Json** package contains APIs that aid in the creation and manipulation of JSON.

    You will build and test your simulated device app in the next task.

1. To ensure all of the application dependencies are downloaded, enter the following command

    ```bash
    dotnet restore
    ```

1. Open **Visual Studio Code**.

1. On the **File** menu, click **Open Folder**.

1. In the **Open Folder** dialog, navigate to the location where you created the **CaveDevice** directory.

1. In the list of folders, click **CaveDevice**, and then click **Select Folder**.

    The EXPLORER pane of Visual Studio Code should now list two C# project files:

    * CaveDevice.csproj
    * Program.cs

    > **Note**: If you see a message **Required assets to build and debug are missing from `CaveDevice`. Add them?**, you may click **Yes** to proceed.

#### Task 3: Explore the application

As noted above, the application currently consists of two files:

* CaveDevice.csproj
* Program.cs

In this task, you will use Visual Studio Code to review the contents and purpose of the two application files.

1. In the **EXPLORER** pane, to open the application project file, click **CaveDevice.csproj**.

    The **CaveDevice.csproj** file should now be opened in the code editor pane.

1. Take a minute to review the contents of the **CaveDevice.csproj** file.

    Your file contents should be similar to the following:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

        <PropertyGroup>
            <OutputType>Exe</OutputType>
            <TargetFramework>netcoreapp3.1</TargetFramework>
        </PropertyGroup>

        <ItemGroup>
            <PackageReference Include="Microsoft.Azure.Devices.Client" Version="1.*" />
            <PackageReference Include="Newtonsoft.Json" Version="12.0.3" />
        </ItemGroup>

    </Project>
    ```

    > **Note**: The package version numbers in your file may differ from those shown above, that's okay.

    The project file (.csproj) is an XML document that specifies the type of project that you are working on. In this case, the project is an **Sdk** style project.

    As you can see, the project definition contains two sections - a **PropertyGroup** and an **ItemGroup**.

    The **PropertyGroup** defines the type of output that building this project will produce. In this case you will be building an executable file that targets .NET Core 3.1.

    The **ItemGroup** specifies any external libraries that are required for the application. These particular references are for NuGet packages, and each package reference specifies the package name and the version. The `dotnet add package` commands (that you entered in the steps above) added these references to the project file and the `dotnet restore` command ensured that all of the dependencies were downloaded.

    > **Information**: You can learn more about NuGet [here](https://docs.microsoft.com/en-us/nuget/what-is-nuget).

1. In the **EXPLORER** pane, click **Program.cs**.

    The **Program.cs** file should now be opened in the code editor pane.

1. Take a minute to review the contents of the **Program.cs** file.

    Your file contents should be similar to the following:

    ```csharp
    using System;

    namespace CaveSensor
    {
        class Program
        {
            static void Main(string[] args)
            {
                Console.WriteLine("Hello World!");
            }
        }
    }
    ```

    This program simply writes "Hello World!" to the command line window. Even though there isn't much code here, there are still some things worth noting:

    * The `using` area - the source file lists the namespaces that the code is **using** (this is typically done at the top of the file as it is here). In this example, the code specifies that it is using `System`. This means that when your code uses a component that's contained within the **System** namespace, you don't have to explicitly list the word **System** within that code line. For example, in the code above, the `Console` class is used to write "Hello World!". The `Console` class is part of the **System** namespace, but you didn't have to include the word `System` when you used `Console`. The benefit of this becomes more apparent when you consider that some namespaces are nested quite deeply (five or more levels is common). Once again referring to the code above, if you didn't specify `using System;`, you would have to write the console line as:

        ```csharp
        System.Console.WriteLine("Hello World!");
        ```

    * The `namespace` area - this specifies that the classes contained with the `{ }` that follow the namespace are part of that namespace. So, similar to how **Console** is part of the **System** namespace, in the example above, the **Program** class is part of the **CaveSensor** namespace, and its full name is **CaveSensor.Program**.

    * The `class` area - this defines the contents of the **Program** class. You can have more than one class within a single source file

    > **Note**: Developers will typically separate classes into their own source file (a single class per source file), especially in larger projects. However, in the labs for this course, you will be including multiple classes per file. This will help to simplify the lab instructions and does not imply best practice.

1. On the Visual Studio Code **View** menu, click **Terminal**.

    This will open the integrated Terminal at the bottom of the Visual Studio Code window. You will be using the Terminal window to compile and run your console application.

1. In the Terminal pane, ensure that the current directory path is set to the `CaveDevice` folder.

    The Terminal command prompt includes the current directory path. The commands that you enter are run at the current location, so be sure that you are located in the `CaveDevice` folder.

1. To build and run the **CaveDevice** project, enter the following command:

    ```cmd/sh
    dotnet run
    ```

1. Notice that **Hello World!** is displayed.

    After a moment, you should see **Hello World!** displayed on the line directly below the `dotnet run` command that you entered.

    You will be using the same `Console.WriteLine` approach in your simulated device application to display information locally, which will help you see the information being sent to IoT Hub and keep track of processes that are being completed by your device. 

    Although this Hello World app demonstrates some basic concepts, it is clearly not a simulated device. In the next task you will replace this code with the code for your simulated device.

#### Task 3: Implement the simulated device code

In this task, you will use Visual Studio Code to enter the code that leverages the Azure IoT Device SDK to connect to your IoT Hub resource.

1. In the **EXPLORER** pane, click **Program.cs**.

1. Select all of the existing code, and then delete it.

1. In the code editor pane, to create the basic structure of your simulated device application, enter the following code:

    ```csharp
    // INSERT using statements below here

    namespace CaveDevice
    {
        class Program
        {
            // INSERT variables below here

            // INSERT Main method below here

            // INSERT SendDeviceToCloudMessagesAsync method below here

            // INSERT CreateMessageString method below here

        }

        // INSERT EnvironmentSensor class below here

    }
    ```

    > **Note:** As you can see, the namespace and the class have been retained, however, the other items are placeholder comments. In the following steps you will be inserting code into the file below specific comments.

1. Locate the `// INSERT using statements below here` comment.

1. To specify the namespaces that the application code will be using, enter the following code:

    ```csharp
    using System;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

    Notice that as well as specifying **System**, you are also declaring other namespaces that the code will be using, such as **System.Text** for encoding strings, **System.Threading.Tasks** for asynchronous tasks, and the namespaces for the two packages you added earlier.

    > **Tip**: When inserting code, the code layout may not be ideal. You can have Visual Studio Code format the document for you by right-clicking in the code editor pane and then clicking **Format Document**. You can achieve the same result by opening the **Task** pane (press **F1**) and typing **Format Document** and then pressing **Enter**. And on Windows, the shortcut for this task is **SHIFT+ALT+F**.

1. Locate the `// INSERT variables below here` comment.

1. To specify the variables that the program is using, enter the following code:

    ```csharp
    // Contains methods that a device can use to send messages to and receive from an IoT Hub.
    private static DeviceClient deviceClient;

    // The device connection string to authenticate the device with your IoT hub.
    // Note: in real-world applications you would not "hard-code" the connection string
    // It could be stored within an environment variable, passed in via the command-line or
    // stored securely within a TPM module.
    private readonly static string connectionString = "{Your device connection string here}";
    ```

1. Take a moment to review the code (and code comments) that you just entered.

    The **deviceClient** variable is used to store an instance of **DeviceClient** - this class comes from the Azure IoT Device SDK and contains methods that a device can use to send messages to and receive from an IoT Hub.

    The **connectionString** variable will contain the connection string for the device we created earlier. This value is used by the **DeviceClient** to connect to the IoT Hub.

    > **Important**: You will see examples in this and other labs throughout this course where connection strings, passwords and other configuration information is hard-coded into the application. This is done solely to simplify the labs and **is not** a recommended practice. As much as possible, security issues like this will be addressed as they come up in the labs. Security topics (and other important considerations) will be addressed during the instructor presentation and in your Student Handbook content in a manner that supports the overall flow of the course. The two may not always align perfectly. As a result, you may be exposed to topics in the labs that are not covered in detail until later in the course.

    As noted within the code comments, connection strings and similar configuration values should be supplied via alternative means such as environment variables, command-line parameters or, better still, stored in secured hardware such as Trusted Platform Modules (TPM).    

1. In the code that you just entered, update the value for **connectionString** using the Primary Connection String that you copied from IoT Hub.

    Once updated, the **connectionString** variable line should be similar to the following:

    ```csharp
    private readonly static string connectionString = "HostName=iot-az220-training-dm200420.azure-devices.net;DeviceId=sensor-th-0001;SharedAccessKey=hfavUmFgoCPA9feWjyfTx23SUHr+dqG9X193ctdEd90=";
    ```

1. Locate the `// INSERT Main method below here` comment.

1. To construct the **Main** method of your simulated device application, enter the following code:

    ```csharp
    private static void Main(string[] args)
    {
        Console.WriteLine("IoT Hub C# Simulated Cave Device. Ctrl-C to exit.\n");

        // Connect to the IoT hub using the MQTT protocol
        deviceClient = DeviceClient.CreateFromConnectionString(connectionString, TransportType.Mqtt);
        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();
    }
    ```

    The **Main** method is the first part of your application that runs once your app is started.

1. Take a minute to review the code (and code comments) that you just entered.

    The basic structure of a simple device app is as follows:

    * Connect to the IoT Hub
    * Send telemetry to the app (Device to Cloud messages)

    Notice that the **deviceClient** variable is initialized with the result of the **DeviceClient** static method, **CreateFromConnectionString**. This method uses the connection string you specified earlier, as well as selecting the protocol that the device will use to the send telemetry -  in this case MQTT.

    > **Note**: In a production application, the **CreateFromConnectionString** method call would be wrapped in exception handling code to gracefully deal with any connection issues. This and other lab code is kept as simple as possible to highlight the key points, so most error-handling is omitted for brevity.

    Once connected, the **SendDeviceToCloudMessagesAsync** method is called. You may notice that the method name is underlined with "red squiggles" - this is because Visual Studio Code has noticed that **SendDeviceToCloudMessagesAsync** is not yet implemented. We will add the method shortly.

    Finally, the application waits for user input.

    > **Information**: The **DeviceClient** class is documented [here](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.devices.client.deviceclient?view=azure-dotnet).
    > 
    > **Information**: The **CreateFromConnectionString** method is documented [here](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.devices.client.deviceclient.createfromconnectionstring?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_CreateFromConnectionString_System_String_Microsoft_Azure_Devices_Client_TransportType_). 
    > 
    > **Information**: The supported transport protocols are documented [here](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-protocols).

1. Locate the `// INSERT - SendDeviceToCloudMessagesAsync below here` comment.

1. To construct the **SendDeviceToCloudMessagesAsync** method, enter the following code:

    ```csharp
    private static async void SendDeviceToCloudMessagesAsync()
    {
        // Create an instance of our sensor
        var sensor = new EnvironmentSensor();

        while (true)
        {
            // read data from the sensor
            var currentTemperature = sensor.ReadTemperature();
            var currentHumidity = sensor.ReadHumidity();

            var messageString = CreateMessageString(currentTemperature, currentHumidity);

            // create a byte array from the message string using ASCII encoding
            var message = new Message(Encoding.ASCII.GetBytes(messageString));

            // Add a custom application property to the message.
            // An IoT hub can filter on these properties without access to the message body.
            message.Properties.Add("temperatureAlert", (currentTemperature > 30) ? "true" : "false");

            // Send the telemetry message
            await deviceClient.SendEventAsync(message);
            Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
        }
    }
    ```

    Notice that the declaration for the **SendDeviceToCloudMessagesAsync** method includes the keyword `async`. This specifies that the method contains asynchronous code that uses the `await` keyword and instructs the compiler to handle the callback plumbing for you.

1. Take a minute to review the code (and code comments) that you just entered.

    This method implements a typical message loop:

    * Read from one or more sensors
    * Create a message to send
    * Send the message
    * Wait for some time, or for an event to occur, etc.
    * Repeat the loop

    The following description explains the method code in more detail:

    * The first thing that your code does is create an instance of the **EnvironmentSensor** class. This is done outside the loop and is used to support simulating the sensor data inside the loop. You will add the **EnvironmentSensor** class shortly.

    * You then start an infinite loop - `while(true) {}` will repeat until the user hits **CTRL+C**.

    * Within the loop, the first thing you do is read the temperature and humidity from your sensor and use those values to create a message string - you will add the code for **CreateMessageString** in a moment as well.

    * Then you create the actual **message** that will be sent to IoT Hub. You do this by creating an instance of the **Message** class from the Azure IoT Device SDK - the data structure that represents the message that is used for interacting with Iot Hub (IoT Hub expects a specific message format). The constructor that you use for the **Message** class requires that the message string be encoded as a byte array.

    * Next, you augment the message with additional properties - here, for example, you set the **temperatureAlert** property to true if the **currentTemperature** is greater than 30, otherwise false.

    * You then send the telemetry message via the `await deviceClient.SendEventAsync(message);` call. Note that this line contains an `await` keyword. This instructs the compiler that the following code is asynchronous and will complete some time in the future - when it does complete, this method will continue executing on the next line.

    * Finally, you write the message string to the local console window to show that telemetry has been sent to IoT Hub, and then wait for 1000 milliseconds (1 second) before repeating the loop.

    > **Information**: You can learn more about `async`, `await` and asynchronous programming in C# [here](https://docs.microsoft.com/en-us/dotnet/csharp/async).

    > **Information**: The **Message** class is documented [here](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.devices.client.message?view=azure-dotnet)

1. Locate the `// INSERT CreateMessageString method below here` comment.

1. To construct the **CreateMessageString** method that creates a JSON string from the sensor readings, enter the following code:

    ```csharp
    private static string CreateMessageString(double temperature, double humidity)
    {
        // Create an anonymous object that matches the data structure we wish to send
        var telemetryDataPoint = new
        {
            temperature = temperature,
            humidity = humidity
        };

        // Create a JSON string from the anonymous object
        return JsonConvert.SerializeObject(telemetryDataPoint);
    }
    ```

    This method creates an anonymous object with the temperature and humidity properties and assigns it to **telemetryDataPoint**.

    The value of **telemetryDataPoint** is then converted to a JSON string via the **JsonConvert** class that is part of the **Newtonsoft.Json** package you added earlier. The JSON string value is then returned to be used as the payload in the message.

1. Locate the `// INSERT EnvironmentSensor class below here` comment.

1. To construct the **EnvironmentSensor** class, enter the following code:

    ```csharp
    /// <summary>
    /// This class represents a sensor 
    /// real-world sensors would contain code to initialize
    /// the device or devices and maintain internal state
    /// a real-world example can be found here: https://bit.ly/IoT-BME280
    /// </summary>
    internal class EnvironmentSensor
    {
        // Initial telemetry values
        double minTemperature = 20;
        double minHumidity = 60;
        Random rand = new Random();

        internal EnvironmentSensor()
        {
            // device initialization could occur here
        }

        internal double ReadTemperature()
        {
            return minTemperature + rand.NextDouble() * 15;
        }

        internal double ReadHumidity()
        {
            return minHumidity + rand.NextDouble() * 20;
        }
    }
    ```

    This is a very simple class that uses random numbers to return values that represent temperature and humidity. In reality, it is often much more complex to interact with sensors, especially if you have to communicate with them at a low-level and derive the measurement value (rather than getting a direct reading in the appropriate units).

    > **Information**: You can view a more representative example of the code that interacts with a simple temperature, humidity and pressure sensor [here](https://bit.ly/IoT-BME280).

1. On the **File** menu, click **Save**.

1. Take a minute to scan through your completed application.

    Your completed application represents a simple simulated device. It demonstrates how to connect a device to an IoT Hub and send Device to Cloud messages. 

    You are now ready to test the application

#### Task 4: Test the application

1. In Visual Studio Code Explorer pane, on the **View** menu, click **Terminal**.

    Verify that the selected terminal shell is the windows command prompt.

1. In the Terminal view, at the command prompt, enter the following command:

    ```cmd/sh
    dotnet run
    ```

    This command will build and run the Simulated Device application. Be sure the terminal location is set to the directory with the `CaveDevice.cs` file.

    > **Note**:  If the command outputs a `Malformed Token` or other error message, then make sure the **Primary Connection String** value is configured correctly as the value of the `connectionString` variable.

    If you receive additional error messages, you can verify that you constructed your code correctly by referring to completed solution code that is available for reference in the **Final** folder for this lab. This **Final** folder is included with the lab resources files that you downloaded when setting up your development environment in lab 3. The folder path is:

    >
    > * Allfiles
    >   * Labs
    >      * LAB_AK_04-connect-iot-device-to-azure
    >        * Final

1. Observe the message string output displayed in the Terminal.

    Once the Simulated Device application is running, it will be sending event messages to the Azure IoT Hub that include `temperature` and `humidity` values, and displaying message string output in the console.

    The terminal output will look similar to the following:

    ```text
    IoT Hub C# Simulated Cave Device. Ctrl-C to exit.

    10/25/2019 6:10:12 PM > Sending message: {"temperature":27.714212817472504,"humidity":63.88147743599558}
    10/25/2019 6:10:13 PM > Sending message: {"temperature":20.017463779085066,"humidity":64.53511070671263}
    10/25/2019 6:10:14 PM > Sending message: {"temperature":20.723927165718717,"humidity":74.07808918230147}
    10/25/2019 6:10:15 PM > Sending message: {"temperature":20.48506045736608,"humidity":71.47250854944461}
    10/25/2019 6:10:16 PM > Sending message: {"temperature":25.027703996760632,"humidity":69.21247714628115}
    10/25/2019 6:10:17 PM > Sending message: {"temperature":29.867399432634656,"humidity":78.19206098010395}
    10/25/2019 6:10:18 PM > Sending message: {"temperature":33.29597232085465,"humidity":62.8990878830194}
    10/25/2019 6:10:19 PM > Sending message: {"temperature":25.77350195766124,"humidity":67.27347029711747}
    ```

    > **Note**: Leave the simulated device app running for now. Your next task will be to verify that your IoT Hub is receiving the telemetry messages.

#### Task 3: Verify Telemetry Stream sent to Azure IoT Hub

In this task, you will use the Azure CLI to verify telemetry sent by the simulated device is being received by Azure IoT Hub.

1. Using a browser, open the [Azure Cloud Shell](https://shell.azure.com/) and login with the Azure subscription you are using for this course.

1. In the Azure Cloud Shell, to monitor the event messages that are being received by your IoT hub, enter the following command:

    ```cmd/sh
    az iot hub monitor-events --hub-name {IoTHubName} --device-id sensor-th-0001
    ```

    _Be sure to replace the **{IoTHubName}** placeholder with the name of your Azure IoT Hub._

    > **Note**:  If you receive a message stating _"Dependency update required for IoT extension version"_ when running the Azure CLI command, then press `y` to accept the update and press `Enter`. This will allow the command to continue as expected.

    The `monitor-events` command (within the `az iot hub` Azure CLI module) offers the capability to monitor device telemetry and other message types sent to an Azure IoT Hub. This can be a very useful tool during code development, and the convenience of the command-line interface is also nice.

    The `--device-id` parameter is optional and allows you to monitor the events from a single device. If the parameter is omitted, the command will monitor all events sent to the specified Azure IoT Hub.

1. Notice that the `az iot hub monitor-events` Azure CLI command outputs a JSON representation of the events that are arriving at your specified Azure IoT Hub. 

    This command enables you to monitor the events being sent to IoT hub. You are also verifying that the device is able to connect to and communicate with the your IoT hub.

    You should see messages displayed that are similar to the following:

    ```cmd/sh
    Starting event monitor, filtering on device: sensor-th-0001, use ctrl-c to stop...
    {
        "event": {
            "origin": "sensor-th-0001",
            "payload": "{\"temperature\":25.058683971901743,\"humidity\":67.54816981383979}"
        }
    }
    {
        "event": {
            "origin": "sensor-th-0001",
            "payload": "{\"temperature\":29.202181296051563,\"humidity\":69.13840303623043}"
        }
    }
    ```

1. Once you have verified that IoT hub is receiving the telemetry, press **Ctrl-C** in the Azure Cloud Shell and Visual Studio Code windows.

    Ctrl-C is used to stop the running apps. Always remember to shut down unneeded apps and jobs.
