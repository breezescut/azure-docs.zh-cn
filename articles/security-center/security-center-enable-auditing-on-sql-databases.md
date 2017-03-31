---
title: "对 Azure 安全中心中的 SQL 数据库启用审核 |Microsoft 文档"
description: "本文档演示如何实现 Azure 安全中心建议**对 SQL 数据库启用审核**。"
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 5d51a5ef3387b4c00079547b0f44ffe1f96bd77c
ms.openlocfilehash: d36bb775d0b0fd732562b18787b65600c8fb5fa1
ms.lasthandoff: 02/17/2017


---
# <a name="enable-auditing-on-sql-databases-in-azure-security-center"></a>对 Azure 安全中心中的 SQL 数据库启用审核
Azure 安全中心将建议对所有 SQL 数据库启用审核（如果尚未启用审核）。 审核可帮助你一直保持遵从法规、了解数据库活动，以及深入了解可以指明业务考量因素或疑似安全违规的偏差和异常。

启用审核后，则可以配置威胁检测设置和电子邮件，以接收安全警报。 威胁检测会检测异常的数据库活动，指出数据库有潜在的安全威胁。 这使你能够检测出潜在威胁，并在威胁发生时做出响应。

此建议仅适用于 Azure SQL 服务；不包括虚拟机上运行的 SQL。

> [!NOTE]
> 本文档将使用示例部署介绍该服务。  这并非一份循序渐进的指南。
>
>

## <a name="implement-the-recommendation"></a>实现该建议
1. 在“建议”边栏选项卡中，选择“对 SQL 数据库启用审核”。  这将打开“对 SQL 数据库启用审核”边栏选项卡。

   ![对 SQL 数据库启用审核][1]
2. 选择要在其上启用审核的 SQL 数据库。 这将打开“审核和威胁检测”边栏选项卡。

   ![审核和威胁检测][2]
3. 在“审核和威胁检测”边栏选项卡上，选择“审核”下方的“ON”。

   ![启用审核和威胁检测][3]
4. 请按照 [Azure 门户中 SQL 数据库威胁检测](../sql-database/sql-database-threat-detection-portal.md)一文中的步骤来启用和配置威胁检测，并配置收到有关检测到异常活动的安全警报的电子邮件列表。

## <a name="see-also"></a>另请参阅
本文档演示如何实现安全中心建议“对 SQL 数据库启用审核。” 若要了解有关保护 SQL 数据库的详细信息，请参阅以下内容：

* [保护 SQL 数据库](../sql-database/sql-database-security-overview.md)

若要了解有关安全中心的详细信息，请参阅以下文章：

* [在 Azure 安全中心中设置安全策略](security-center-policies.md)了解如何配置 Azure 订阅和资源组的安全策略。
* [在 Azure 安全中心中管理安全建议](security-center-recommendations.md) -- 了解建议如何帮助保护 Azure 资源。
* [Azure 安全中心的安全性运行状况监视](security-center-monitoring.md) -- 了解如何监视 Azure 资源的运行状况。
* [管理和响应 Azure 安全中心的安全警报](security-center-managing-and-responding-alerts.md) -- 了解如何管理和响应安全警报。
* [通过 Azure 安全中心监视合作伙伴解决方案](security-center-partner-solutions.md) -- 了解如何监视合作伙伴解决方案的运行状态。
* [Azure 安全中心常见问题解答](security-center-faq.md)查找有关使用服务的常见问题。
* [Azure 安全博客](http://blogs.msdn.com/b/azuresecurity/)获取最新的 Azure 安全新闻和信息。

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]:./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
