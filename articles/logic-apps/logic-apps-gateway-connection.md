---
title: "访问本地数据 - Azure 逻辑应用 | Microsoft 文档"
description: "逻辑应用可以通过连接到本地数据网关来访问本地数据。"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 6cb4449d-e6b8-4c35-9862-15110ae73e6a
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/05/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: ce0be184fe11a7e5873639d79961e98de730ec86
ms.lasthandoff: 03/10/2017


---
# <a name="connect-to-on-premises-data-from-logic-apps"></a>从逻辑应用连接到本地数据

若要访问本地数据，可与受支持的 Azure 逻辑应用连接器的本地数据网关建立连接。 以下步骤逐步说明如何安装和设置本地数据网关，以便能够使用逻辑应用。
本地数据网关支持以下数据源连接：

*   BizTalk Server
*    DB2  
*   文件系统
*   Informix
*   MQ
*    Oracle Database 
*   SAP Application Server 
*   SAP Message Server
*    SQL Server

有关这些连接的详细信息，请参阅 [Connectors for Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)（适用于 Azure 逻辑应用的连接器）。

## <a name="requirements"></a>要求

* 必须在 Azure 中使用工作或学校电子邮件地址将本地数据网关与帐户（基于 Azure Active Directory 的帐户）相关联。

* 如果使用 Microsoft 帐户（例如 @outlook.com），可以使用 Azure 帐户[创建工作或学校电子邮件地址](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal)。

* 必须事先[在本地计算机上安装本地数据网关](logic-apps-gateway-install.md)。

* 只能将安装关联到一个网关资源。 网关不能已由另一个 Azure 本地数据网关声明。 声明将在[本主题的步骤 2 的创建过程中](#2-create-an-azure-on-premises-data-gateway-resource)进行。

## <a name="install-and-configure-the-connection"></a>安装和配置连接

### <a name="1-install-the-on-premises-data-gateway"></a>1.安装本地数据网关

遵循以下步骤[安装本地数据网关](logic-apps-gateway-install.md)（如果尚未安装）。 在继续执行其他步骤之前，请确保在本地计算机上安装数据网关。

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2.创建 Azure 本地数据网关资源

安装网关后，必须将 Azure 订阅与网关相关联。

1. 使用安装网关期间使用的同一工作或学校电子邮件地址登录到 Azure。
2. 选择“新建”。
3. 找到并选择“本地数据网关”。
4. 若要将网关与帐户相关联，请填写相关的信息，包括选择相应的“安装名称”。
   
    ![本地数据网关连接][1]

5. 若要创建该资源，请选择“创建”。

### <a name="3-create-a-logic-app-connection-in-logic-app-designer"></a>3.在逻辑应用设计器中创建逻辑应用连接

将 Azure 订阅与本地数据网关的实例相关联后，可以通过逻辑应用来与该网关建立连接。

1. 打开逻辑应用，然后选择支持本地连接的连接器，例如 SQL Server。
2. 选择“通过本地数据网关连接”。
   
    ![逻辑应用设计器网关创建][2]

3. 选择要连接到的**网关**，然后填写所需的其他所有连接信息。
4. 若要创建连接，请选择“创建”。

现已配置可供逻辑应用使用的连接。

## <a name="next-steps"></a>后续步骤

* [逻辑应用的常见示例和方案](../logic-apps/logic-apps-examples-and-scenarios.md)
* [企业集成功能](../logic-apps/logic-apps-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/logic-apps-gateway-connection/createblade.png
[2]: ./media/logic-apps-gateway-connection/blankconnection.png
[3]: ./media/logic-apps-logic-gateway-connection/checkbox.png

