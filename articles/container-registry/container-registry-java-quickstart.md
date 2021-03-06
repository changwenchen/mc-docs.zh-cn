---
title: 快速入门 - 使用 Maven 和 Jib 生成 Java 容器映像并将其推送到 Azure 容器注册表
description: 使用 Maven Jib 插件生成容器化 Java 应用并将其推送到 Azure 容器注册表。
author: rockboyfor
ms.topic: quickstart
origin.date: 02/26/2020
ms.date: 05/08/2020
ms.author: v-yeche
ms.openlocfilehash: 888dbde9f23641fd50a1e926d766bd9d89c2f198
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83001999"
---
<!--No exception in Prerequistes on Azure China Cloud-->
# <a name="quickstart-build-and-push-java-container-images-to-azure-container-registry"></a>快速入门：生成 Java 容器映像并将其推送到 Azure 容器注册表

本快速入门介绍如何使用 Maven Jib 插件生成容器化 Java 应用，并将其推送到 Azure 容器注册表。 Maven 和 Jib 是开发人员在与 Azure 容器注册表交互时可用的工具的一个示例。

## <a name="prerequisites"></a>先决条件

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益](https://www.azure.cn/offers/ms-mc-arz-msdn/index.html)或注册 [Azure 试用帐户](https://www.azure.cn/pricing/free-trial)。
* [Azure 命令行接口 (CLI)](https://docs.azure.cn/cli/overview?view=azure-cli-latest)。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://docs.azure.cn/java/java-supported-jdk-runtime?view=azure-java-stable>。
* Apache 的 [Maven](http://maven.apache.org) 生成工具（版本 3 或以上）。
* [Git](https://git-scm.com) 客户端。
* [Docker](https://www.docker.com) 客户端。
* [ACR Docker 凭据帮助器](https://github.com/Azure/acr-docker-credential-helper)。

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>创建 Docker 上的 Spring Boot 入门 Web 应用

以下步骤将指导用户构建 Spring Boot web 应用程序并在本地进行测试。

1. 在命令提示符下，使用以下命令克隆 [Docker 上的 Spring Boot 入门](https://github.com/spring-guides/gs-spring-boot-docker)示例项目。

    ```bash
    git clone https://github.com/spring-guides/gs-spring-boot-docker.git
    ```

1. 将目录更改为已完成项目。

    ```bash
    cd gs-spring-boot-docker/complete
    ```

1. 使用 Maven 生成和运行示例应用。

    ```bash
    mvn package spring-boot:run
    ```

1. 通过浏览到 `http://localhost:8080` 或使用以下 `curl` 命令测试 Web 应用：

    ```bash
    curl http://localhost:8080
    ```

应当会看到显示了以下消息：**Hello Docker World**

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>使用 Azure CLI 创建 Azure 容器注册表

接下来，使用以下步骤创建 Azure 资源组和 ACR：

1. 使用以下命令登录到 Azure 帐户：

    ```azurecli
    az login
    ```

1. 指定要使用的 Azure 订阅：

    ```azurecli
    az account set -s <subscription ID>
    ```

1. 为本教程中使用的 Azure 资源创建资源组。 在以下命令中，请务必将占位符替换为自己的资源名称和位置，例如 `chinanorth`。

    ```azurecli
    az group create \
       --name=<your resource group name> \
       --location=<location>
    ```

1. 使用以下命令在资源组中创建专用 Azure 容器注册表。 请务必将占位符替换为实际值。 本教程的后续步骤会将示例应用作为 Docker 映像推送到此注册表。

    ```azurecli
    az acr create \
       --resource-group <your resource group name> \
       --location <location> \
       --name <your registry name> \
       --sku Basic
    ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a>通过 Jib 将应用推送到容器注册表

最后，请更新项目配置，使用命令提示符生成并部署映像。

1. 在 Azure CLI 中使用以下命令登录到 Azure 容器注册表。 请务必将占位符替换为自己的注册表名称。

    ```azurecli
    az configure --defaults acr=<your registry name>
    az acr login
    ```

    `az configure` 命令设置要在 `az acr` 命令中使用的默认注册表名称。

1. 导航到 Spring Boot 应用程序的完整项目目录（例如，“C:\SpringBoot\gs-spring-boot-docker\complete”或“/users/robert/SpringBoot/gs-spring-boot-docker/complete”），并使用文本编辑器打开 pom.xml 文件    。

1. 使用以下 XML 更新 *pom.xml* 文件中的 `<properties>` 集合。 请将占位符替换为自己的注册表名称，并使用最新的 [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) 版本更新 `<jib-maven-plugin.version>` 值。

    ```xml
    <properties>
      <docker.image.prefix><your registry name>.azurecr.cn</docker.image.prefix>
      <jib-maven-plugin.version>1.8.0</jib-maven-plugin.version>
      <java.version>1.8</java.version>
    </properties>
    ```

1. 更新 *pom.xml* 文件中的 `<plugins>` 集合，使 `<plugin>` 元素包含 `jib-maven-plugin` 的条目，如以下示例中所示。 请注意，我们将使用 Microsoft 容器注册表 (MCR) 中的基础映像：`mcr.microsoft.com/java/jdk:8-zulu-alpine`，其中包含 Azure 正式支持的 JDK。 有关包含正式支持的 JDK 的其他 MCR 基础映像，请参阅 [Java SE JDK](https://hub.docker.com/_/microsoft-java-jdk)、[Java SE JRE](https://hub.docker.com/_/microsoft-java-jre)、[Java SE 无头 JRE](https://hub.docker.com/_/microsoft-java-jre-headless) 以及 [Java SE JDK 和 Maven](https://hub.docker.com/_/microsoft-java-maven)。
    
    <!--CORRECT ON Microsoft Container Registry-->
     
    ```xml
    <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>mcr.microsoft.com/java/jdk:8-zulu-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
        </to>
     </configuration>
    </plugin>
    ```

1. 导航到 Spring Boot 应用程序的完成项目目录，然后运行以下命令以生成映像并将映像推送到注册表：

    ```bash
    mvn compile jib:build
    ```

> [!NOTE]
>
> 出于安全原因，`az acr login` 创建的凭据的有效期仅为 1 小时。 如果收到“401 未授权”错误，可以再次运行 `az acr login -n <your registry name>` 命令以重新进行身份验证。 

## <a name="verify-your-container-image"></a>验证容器映像

祝贺你！ 现已在 Azure 支持的 JDK 中生成了容器化 Java 应用，并已将其推送到 ACR。 接下来可通过以下方式测试映像：将其部署到 Azure 应用服务，或使用以下命令（请替换占位符）将其提取到本地：

```bash
docker pull <your registry name>.azurecr.cn/gs-spring-boot-docker:v1
```

## <a name="next-steps"></a>后续步骤

有关 Microsoft 正式支持的其他 Java 基础映像版本，请参阅：

* [Java SE JDK](https://hub.docker.com/_/microsoft-java-jdk)
* [Java SE JRE](https://hub.docker.com/_/microsoft-java-jre)
* [Java SE 无头 JRE](https://hub.docker.com/_/microsoft-java-jre-headless)
* [Java SE JDK 和 Maven](https://hub.docker.com/_/microsoft-java-maven)

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](https://docs.azure.cn/java/spring-framework)

### <a name="additional-resources"></a>其他资源

有关详细信息，请参阅以下资源：

* [面向 Java 开发人员的 Azure](https://docs.azure.cn/java/?view=azure-java-stable)
    
    <!--Not Available on * [Working with Azure DevOps and Java](/azure/devops/java)-->
    
* [Docker 上的 Spring Boot 入门](https://spring.io/guides/gs/spring-boot-docker)
* [Spring Initializr](https://start.spring.io)
* [将 Spring Boot 应用程序部署到 Azure 应用服务](https://docs.microsoft.com/azure/java/spring-framework/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin)

    <!--Not Available on * [Using a custom Docker image for Azure Web App on Linux](/app-service-web/app-service-linux-using-custom-docker-image)-->

<!-- Update_Description: new article about container registry java quickstart -->
<!--NEW.date: 03/09/2020-->