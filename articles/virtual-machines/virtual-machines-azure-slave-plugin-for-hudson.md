---
title: "如何在 Hudson 持续集成中使用 Azure 从属插件 | Microsoft Docs"
description: "介绍如何在 Hudson 连续集成中使用 Azure 从属插件。"
services: virtual-machines-linux
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: ff60ebaddd3a7888cee612f387bd0c50799496ac
ms.openlocfilehash: 0661f26e62a465ccd096773cbabd47268491f22d


---
# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>如何在 Hudson 连续集成中使用 Azure 从属插件
适用于 Hudson 的 Azure 从属插件让你能够在运行分布式构建系统时，预配 Azure 上的从属节点。

## <a name="install-the-azure-slave-plug-in"></a>安装 Azure 从属插件
1. 在 Hudson 仪表板中，单击“管理 Hudson”。
2. 在“管理 Hudson”页面中，单击“管理插件”。
3. 单击“可用”选项卡。
4. 单击“搜索”，然后键入 **Azure**，将列表限制到相关插件。
   
    如果选择滚动可用插件列表，会在“其他”选项卡中的“群集管理和分布式构建”部分下找到 Azure 从属插件。
5. 选中“Azure 从属插件”复选框。
6. 单击“安装” 。
7. 重新启动 Hudson。

现在插件安装完毕，接下来的步骤是用 Azure 订阅配置文件配置插件并创建一个在为从属节点创建 VM 时要使用的模板。

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>使用订阅配置文件配置 Azure 从属插件
订阅配置文件也称为发布设置，是一个 XML 文件，包含在开发环境中使用 Azure 时需要的安全凭据以及一些附加信息。 若要配置 Azure 从属插件，需要：

* 订阅 ID
* 订阅的管理证书

可以在[订阅配置文件]中找到这些信息。 下面是订阅配置文件的一个示例。

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

          <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

              ServiceManagementUrl="https://management.core.windows.net"

              Id="<Subscription ID>"

              Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

          </PublishProfile>

    </PublishData>

获取订阅配置文件后，请按照以下步骤配置 Azure 从属插件。

1. 在 Hudson 仪表板中，单击“管理 Hudson”。
2. 单击“配置系统”。
3. 向下滚动页面找到“云”部分。
4. 单击“添加新云”>“Microsoft Azure”。
   
    ![添加新的云][add new cloud]
   
    此时将显示用于输入订阅详细信息的字段。
   
    ![配置配置文件][configure profile]
5. 从订阅配置文件中复制订阅 ID 和管理证书，然后将其粘贴到相应的字段中。
   
    复制订阅 ID 和管理证书时，请**不要**包含将值括起来的引号。
6. 单击“验证配置”。
7. 配置成功通过验证后，单击“保存”。

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>为 Azure 从属插件设置虚拟机模板
虚拟机模板定义插件在 Azure 上创建从属节点时使用的参数。 在以下步骤中，我们将为一个 Ubuntu VM 创建模板。

1. 在 Hudson 仪表板中，单击“管理 Hudson”。
2. 单击“配置系统”。
3. 向下滚动页面找到“云”部分。
4. 在“云”部分中，找到“添加 Azure 虚拟机模板”，然后单击“添加”按钮。
   
    ![添加 VM 模板][add vm template]
5. 在“名称”字段中指定一个云服务名称。 如果你指定的名称引用一个现有的云服务，则将在该服务中设置 VM。 否则，Azure 将创建一个新服务。
6. 在“说明”字段中，输入描述所创建模板的文本。 此信息仅用于记录目的，并不在设置 VM 时使用。
7. 在“标签”字段中，输入 **linux**。 此标签用于标识所创建的模板，随后在创建 Hudson 作业时用于引用该模板。
8. 选择要创建 VM 的所在区域。
9. 选择适当的 VM 大小。
10. 指定要创建 VM 的存储帐户。 确保其所在区域与你要使用的云服务相同。 如果你想要创建新的存储空间，可以将此字段留空。
11. 保留时间指定 Hudson 删除空闲从属节点之前所需要的分钟数。 其保留默认值为 60。
12. 在“使用情况”中，选择使用此从属节点时的适当条件。 现在，选择“尽可能多地使用此节点”。
    
     此时，你的窗体会与以下类似：
    
     ![模板配置][template config]
13. 在“映像系列或 ID”中，必须指定要在 VM 中安装的系统映像。 可以从映像系列的列表中选择，也可以指定一个自定义映像。
    
     如果要从映像系列的列表中选择，请输入映像系列名称的首字符（区分大小写）。 例如，键入 **U** 将显示一个 Ubuntu Server 系列列表。 从列表中选择后，Jenkins 将在设置你的 VM 时使用该系列的最新版系统映像。
    
     ![OS 系列列表][OS family list]
    
     如果你有一个自定义映像，想要改用该映像，则输入该自定义映像的名称。 自定义映像名称不显示在列表中，因此你必须确保名称输入正确。    
    
     对于本教程，键入 **U** 显示一个 Ubuntu 映像列表，然后选择“Ubuntu Server 14.04 LTS”。
14. 对于“启动方法”，请选择“SSH”。
15. 复制下面的脚本并粘贴到“初始化脚本”字段中。
    
         # Install Java
    
         sudo apt-get -y update
    
         sudo apt-get install -y openjdk-7-jdk
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y openjdk-7-jdk
    
         # Install git
    
         sudo apt-get install -y git
    
         #Install ant
    
         sudo apt-get install -y ant
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y ant
    
     将在创建 VM 后执行“初始化脚本”。 在此示例中，脚本安装 Java、git 和 ant。
16. 在“用户名”和“密码”字段中，为要在 VM 中创建的管理员帐户创建首选值。
17. 单击“验证模板”检查指定的参数是否有效。
18. 单击“保存” 。

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>创建在 Azure 上的从属节点上运行的 Hudson 作业
在此部分，你将创建一个在 Azure 上的从属节点上运行的 Hudson 任务。

1. 在 Hudson 仪表板中，单击“新建作业”。
2. 为你正在创建的作业输入一个名称。
3. 对于作业类型，选择“生成自由格式的软件作业”。
4. 单击 **“确定”**。
5. 在作业配置页中，选择“限制可以运行此项目的位置”。
6. 选择“节点和标签菜单”，然后选择 **linux**（在上一部分中创建虚拟机模板时，我们指定了此标签）。
7. 在“构建”部分，单击“添加构建步骤”，然后选择“执行 shell”。
8. 编辑以下脚本，用相应的值替换 **{your github account name}**、**{your project name}** 和 **{your project directory}**，然后将编辑后的脚本粘贴到显示的文本区域中。
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory to project
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. 单击“保存” 。
10. 在 Hudson 仪表板中，找到刚刚创建的作业，然后单击“计划生成”图标。

之后，Hudson 将使用在上一部分中创建的模板创建一个从属节点，并执行你在此任务的构建步骤中指定的脚本。

## <a name="next-steps"></a>后续步骤
有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心]。

<!-- URL List -->

[Azure Java 开发人员中心]: https://azure.microsoft.com/develop/java/
[订阅配置文件]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png




<!--HONumber=Jan17_HO1-->


