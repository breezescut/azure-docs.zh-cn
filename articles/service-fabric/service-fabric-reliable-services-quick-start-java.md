---
title: "在 Java 中创建第一个可靠 Azure 微服务 | Microsoft Docs"
description: "介绍如何创建具有无状态服务和有状态服务的 Microsoft Azure Service Fabric 应用程序。"
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2017
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: cf8f717d5343ae27faefdc10f81b4feaccaa53b9
ms.openlocfilehash: 5a29d6838af7f3952ad96158e5962b17c0f4cb6b


---
# <a name="get-started-with-reliable-services"></a>Reliable Services 入门
> [!div class="op_single_selector"]
> * [Windows 上的 C#](service-fabric-reliable-services-quick-start.md)
> * [Linux 上的 Java](service-fabric-reliable-services-quick-start-java.md)
>
>

本文介绍 Azure Service Fabric Reliable Services 的基础知识，并演示如何创建和部署以 Java 编写的简单 Reliable Service 应用程序。 此 Microsoft Virtual Academy 视频还演示了如何创建无状态可靠服务：<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a>安装和设置
在开始之前，请确保已在计算机上设置 Service Fabric 开发环境。
如果需要设置环境，请转到[在 Mac 上开始使用](service-fabric-get-started-mac.md)或[在 Linux 上开始使用](service-fabric-get-started-linux.md)。

## <a name="basic-concepts"></a>基本概念
若要开始使用 Reliable Services，只需了解几个基本概念：

* **服务类型**：这是服务实现。 它由你编写的可扩展 `StatelessService` 的类、其中使用的任何其他代码或依赖项以及名称和版本号定义。
* **命名服务实例**：若要运行服务，需要创建服务类型的命名实例，就像创建类类型的对象实例一样。 事实上，服务实例是编写的服务类的对象实例化。
* **服务宿主**：创建的命名服务实例需在宿主中运行。 服务宿主是可以运行服务实例的进程。
* **服务注册**：通过注册可将所有对象融合在一起。 只有将服务类型注册到服务宿主中的 Service Fabric 运行时后，Service Fabric 才能创建该类型的可运行实例。  

## <a name="create-a-stateless-service"></a>创建无状态服务
首先创建 Service Fabric 应用程序。 适用于 Linux 的 Service Fabric SDK 包括一个 Yeoman 生成器，它为包含无状态服务的 Service Fabric 应用程序提供基架。 首先，请运行以下 Yeoman 命令：

```bash
$ yo azuresfjava
```

按照说明创建**可靠无状态服务**。 本教程将应用程序命名为“HelloWorldApplication”，将服务命名为“HelloWorld”。 结果包含 `HelloWorldApplication` 和 `HelloWorld` 的目录。

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>实现服务
打开 **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**。 此类定义服务类型，可以运行任何代码。 服务 API 为你的代码提供两个入口点：

* 名为 `runAsync()` 的开放式入口点方法，可在其中开始执行任何工作负荷，包括长时间运行的计算工作负荷。

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* 一个通信入口点，可在其中插入选择的通信堆栈。 这就是你可以开始接收来自用户和其他服务请求的位置。

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

在本教程中，我们将重点放在 `runAsync()` 入口点方法上。 这是你可以立即开始运行代码的位置。

### <a name="runasync"></a>RunAsync
当服务实例已放置并且可以执行时，平台将调用此方法。 对于无状态服务，这就意味着打开服务实例。 需要关闭服务实例时，将提供取消标记进行协调。 在 Service Fabric 中，服务实例的此打开-关闭循环可能会在服务的整个生存期内出现多次。 发生这种情况的原因多种多样，包括：

* 系统可能会移动服务实例以实现资源平衡。
* 代码中发生错误。
* 应用程序或系统升级。
* 基础硬件遇到中断。

Service Fabric 将管理此业务流程，以便保持服务的高度可用和适当均衡。

`runAsync()` 不应阻止同步。 runAsync 的实现应返回一个 CompletableFuture，以允许运行时继续执行。 如果工作负荷需要实现一个应该在 CompletableFuture 内完成的、长期运行的任务。

#### <a name="cancellation"></a>取消
取消工作负荷是一项由所提供的取消标记协调的协同操作。 系统会等任务结束后（成功完成、取消或出现故障）再执行下一步操作。 当系统请求取消时，请务必接受取消标记，完成所有任务，然后尽快退出 `runAsync()`。 以下示例演示如何处理取消事件：

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace the following sample code with your own logic
        // or remove this runAsync override if it's not needed in your service.

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
```

### <a name="service-registration"></a>服务注册
必须将服务类型注册到 Service Fabric 运行时。 服务类型在 `ServiceManifest.xml` 中以及实现 `StatelessService` 的服务类中定义。 服务注册在进程主入口点中执行。 在本示例中，进程主入口点为 `HelloWorldServiceHost.java`：

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="run-the-application"></a>运行应用程序
Yeoman 基架包含一个用于构建应用程序的 gradle 脚本，以及一个用于部署和取消部署应用程序的 bash 脚本。 若要运行应用程序，请先使用 gradle 构建应用程序：

```bash
$ gradle
```

这会生成可以使用 Service Fabric Azure CLI 部署的 Service Fabric 应用程序包。 Install.sh 脚本包含用于部署应用程序包的 Azure CLI 命令。 运行 install.sh 脚本进行部署：

```bash
$ ./install.sh
```



<!--HONumber=Jan17_HO4-->


