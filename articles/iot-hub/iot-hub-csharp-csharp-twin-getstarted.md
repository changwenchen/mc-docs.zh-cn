---
title: Azure IoT 中心设备孪生入门 (.NET/.NET) | Azure
description: 如何使用 Azure IoT 中心设备孪生添加标记，并使用 IoT 中心查询。 使用适用于 .NET 的 Azure IoT 设备 SDK 实现模拟设备应用，并使用适用于 .NET 的 Azure IoT 服务 SDK 实现可添加标记并运行 IoT 中心查询的服务应用。
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
origin.date: 08/26/2019
ms.author: v-yiso
ms.date: 03/09/2020
ms.openlocfilehash: 26a478959cc2b9a8d0de2b4277303320e4f4efcb
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78155095"
---
# <a name="get-started-with-device-twins-net"></a>设备孪生入门 (.NET)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

在本教程中，你将创建两个 .NET 控制台应用：

* **CreateDeviceIdentity**。 此应用创建设备标识和关联的安全密钥，以连接模拟设备应用。

* **AddTagsAndQuery**。 此后端应用添加标记并查询设备孪生。

* **ReportConnectivity**。 此设备应用模拟使用以前创建的设备标识连接到 IoT 中心的设备，并报告其连接状态。

> [!NOTE]
> [Azure IoT SDK](iot-hub-devguide-sdks.md) 一文介绍了可用于构建设备和后端应用的 Azure IoT SDK。
>

## <a name="prerequisites"></a>必备条件

* Visual Studio。
* 有效的 Azure 帐户。 （如果没有帐户，只需几分钟即可创建一个[试用帐户][lnk-free-trial]。）
* 确保已在防火墙中打开端口 8883。 本文中的设备示例使用 MQTT 协议，该协议通过端口 8883 进行通信。 在某些公司和教育网络环境中，此端口可能被阻止。 有关解决此问题的更多信息和方法，请参阅[连接到 IoT 中心(MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)。

## <a name="create-an-iot-hub"></a>创建 IoT 中心

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>在 IoT 中心内注册新设备

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>获取 IoT 中心连接字符串

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>创建服务应用

在本部分，你将使用 C# 创建一个 .NET 控制台应用，用于将位置元数据添加到与 **myDeviceId** 关联的设备孪生。 然后，该应用将选择位于美国的设备来查询存储在 IoT 中心的设备孪生，然后查询报告手机网络连接的设备孪生。

1. 在 Visual Studio 中选择“创建新项目”  。 在“创建新项目”中，选择“控制台应用(.NET Framework)”，然后选择“下一步”    。

1. 在“配置新项目”中，将项目命名为 **AddTagsAndQuery**。 

    ![配置 AddTagsAndQuery 项目](./media/iot-hub-csharp-csharp-twin-getstarted/config-addtagsandquery-app.png)

1. 在解决方案资源管理器中，右键单击“AddTagsAndQuery”  项目，并选择“管理 NuGet 包”  。

1. 选择“浏览”，然后搜索并选择 **Microsoft.Azure.Devices**。  选择“安装”  。

    ![“NuGet 包管理器”窗口](./media/iot-hub-csharp-csharp-twin-getstarted/nuget-package-addtagsandquery-app.png)

   此步骤将下载、安装 [Azure IoT 服务 SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet 包及其依赖项并添加对其的引用。

1. 在 Program.cs`using`**文件顶部添加以下** 语句：

    ```csharp  
    using Microsoft.Azure.Devices;
    ```

1. 将以下字段添加到 Program 类  。 将 `{iot hub connection string}` 替换为在[获取 IoT 中心连接字符串](#get-the-iot-hub-connection-string)中复制的 IoT 中心连接字符串。

    ```csharp  
    static RegistryManager registryManager;
    static string connectionString = "{iot hub connection string}";
    ```

6. 将以下方法添加到 **Program** 类：

    ```csharp  
    public static async Task AddTagsAndQuery()
    {
        var twin = await registryManager.GetTwinAsync("myDeviceId");
        var patch =
            @"{
                tags: {
                    location: {
                        region: 'US',
                        plant: 'Redmond43'
                    }
                }
            }";
        await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
        var query = registryManager.CreateQuery(
          "SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
        var twinsInRedmond43 = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43: {0}", 
          string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
        query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
        var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43 using cellular network: {0}", 
          string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
    }
    ```
   
    **RegistryManager** 类公开从该服务与设备孪生交互所需的所有方法。 上面的代码首先初始化 **registryManager** 对象，并检索 **myDeviceId** 的设备孪生，最后使用所需位置信息更新其标记。
   
    在更新后，它将执行两个查询：第一个仅选择位于 **Redmond43** 工厂的设备的设备孪生，第二个将查询细化为仅选择还要通过移动电话网络连接的设备。

    上面的代码创建 query 对象时，会指定返回的最大文档数  。 **query** 对象包含 **HasMoreResults** 布尔值属性，可以使用它多次调用 **GetNextAsTwinAsync** 方法来检索所有结果。 名为 **GetNextAsJson** 的方法可用于非设备孪生的结果（例如聚合查询的结果）。

7. 最后，在 **Main** 方法中添加以下行：

    ```csharp  
    registryManager = RegistryManager.CreateFromConnectionString(connectionString);
    AddTagsAndQuery().Wait();
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

1. 右键单击 **AddTagsAndQuery** 项目并选择“调试”  ，选择“启动新实例”  来运行此应用程序。 在查询位于 **Redmond43** 的所有设备的查询结果中，应该会看到一个设备，而在将结果限制为使用蜂窝网络的设备的查询结果中没有任何设备。

    ![在窗口中查询结果](./media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png)

在下一部分中，创建的设备应用将报告连接信息，并更改上一部分中查询的结果。

## <a name="create-the-device-app"></a>创建设备应用
本部分中会创建一个 .NET 控制台应用，该应用以 myDeviceId 连接到中心，然后更新其报告属性，使报告中包含它使用移动电话网络进行连接的信息  。

1. 在 Visual Studio 中，选择“文件”   > “新建”   > “项目”  。 在“创建新项目”中，选择“控制台应用(.NET Framework)”，然后选择“下一步”    。

1. 在“配置新项目”中，将项目命名为 **ReportConnectivity**。  对于“解决方案”，请选择“添加到解决方案”，然后选择“创建”。   

1. 在解决方案资源管理器中，右键单击“ReportConnectivity”  项目，并选择“管理 NuGet 包”  。

1. 选择“浏览”，然后搜索并选择 **Microsoft.Azure.Devices.Client**。  选择“安装”  。

   此步骤将下载、安装 [Azure IoT 设备 SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet 包及其依赖项并添加对它的引用。

1. 在 Program.cs`using`**文件顶部添加以下** 语句：

    ```csharp  
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

1. 将以下字段添加到 Program 类  。 将 `{device connection string}` 替换为在[在 IoT 中心注册新设备](#register-a-new-device-in-the-iot-hub)中记下的设备连接字符串。

    ```csharp  
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.cn;
      DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. 将以下方法添加到 **Program** 类：

    ```csharp
    public static async void InitClient()
    {
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
              TransportType.Mqtt);
            Console.WriteLine("Retrieving twin");
            await Client.GetTwinAsync();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

    **Client** 对象公开从该设备与设备孪生交互所需的所有方法。 上述代码会初始化“客户端”  对象，然后检索 **myDeviceId** 设备孪生。

7. 将以下方法添加到 **Program** 类：

    ```csharp  
    public static async void ReportConnectivity()
    {
        try
        {
            Console.WriteLine("Sending connectivity data as reported property");
            
            TwinCollection reportedProperties, connectivity;
            reportedProperties = new TwinCollection();
            connectivity = new TwinCollection();
            connectivity["type"] = "cellular";
            reportedProperties["connectivity"] = connectivity;
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

   上述代码会使用连接信息更新 **myDeviceId** 的报告属性。

8. 最后，在 **Main** 方法中添加以下行：

    ```csharp
    try
    {
        InitClient();
        ReportConnectivity();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

1. 在解决方案资源管理器中，右键单击解决方案并选择“设置启动项目”。 

1. 在“通用属性” **“启动项目”中，选择“多个启动项目”。**  >    对于“ReportConnectivity”，请选择“启动”作为“操作”。    选择“确定”  保存更改。  

1. 右键单击“ReportConnectivity”  项目并选择“调试”  ，然后选择“启动新实例”  来运行此应用。 随后应会看到该应用获取孪生信息，然后发送连接信息作为报告属性。

    ![运行设备应用以报告连接性](./media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png)

   设备报告了其连接信息后，应出现在两个查询中。

1. 右键单击“AddTagsAndQuery”项目并选择“调试” **“启动新实例”，以再次运行查询。**   >   这次，**myDeviceId** 应出现在两个查询结果中。

    ![设备连接性报告成功](./media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png)

## <a name="next-steps"></a>后续步骤
在本教程中，你已在 Azure 门户中配置了新的 IoT 中心，并在 IoT 中心的标识注册表中创建了设备标识。 已从后端应用以标记形式添加了设备元数据，并编写了模拟的设备应用，用于报告设备孪生中的设备连接信息。 还学习了如何使用类似于 SQL 的 IoT 中心查询语言查询此信息。

可通过以下资源了解详细信息：

* 若要了解如何从设备发送遥测数据，请参阅[从设备将遥测数据发送到 IoT 中心](quickstart-send-telemetry-dotnet.md)教程。

* 若要了解如何使用设备孪生的所需属性配置设备，请参阅[使用所需属性配置设备](tutorial-device-twins.md)教程。

* 若要了解如何以交互方式控制设备（例如从用户控制的应用打开风扇），请参阅[使用直接方法](quickstart-control-device-dotnet.md)教程。



<!-- images -->
[img-servicenuget]: ./media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png
[img-createapp]: ./media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png
[img-addtagapp]: ./media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png
[img-rundeviceapp]: ./media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png
[img-tagappsuccess]: ./media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png

<!-- links -->
[lnk-hub-sdks]: ./iot-hub-devguide-sdks.md
[lnk-free-trial]: https://www.azure.cn/pricing/1rmb-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/

[lnk-device-identity-csharp]: ./iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp
[lnk-d2c]: ./iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ./iot-hub-devguide-direct-methods.md
[lnk-twins]: ./iot-hub-devguide-device-twins.md
[lnk-query]: ./iot-hub-devguide-query-language.md
[lnk-identity]: ./iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: quickstart-send-telemetry-dotnet.md
[lnk-methods-tutorial]: quickstart-control-device-node.md
[lnk-twin-how-to-configure]: tutorial-device-twins.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

