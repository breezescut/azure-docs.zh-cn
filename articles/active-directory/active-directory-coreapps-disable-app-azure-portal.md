---
title: "在 Azure Active Directory 预览版中对企业应用禁用用户登录 | Microsoft Docs"
description: "如何禁用企业应用程序，防止用户在 Azure Active Directory 中登录该程序"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: c57505d037d991f3c0d720a481393ec2ae6b8069
ms.openlocfilehash: ef6ae32c1d9b2e417d2a766207575f19aa06feaa


---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>在 Azure Active Directory 预览版中对企业应用禁用用户登录
可以轻松地禁用企业应用程序，防止用户在 Azure Active Directory (Azure AD) 预览版中登录该程序。 [预览包括哪些内容？](active-directory-preview-explainer.md) 用户必须具有相应的权限才能管理企业应用。 在当前预览版中，用户必须是目录的全局管理员。

## <a name="how-do-i-disable-user-sign-ins"></a>如何禁用用户登录？
1. 使用目录全局管理员的帐户登录到 [Azure 门户](https://portal.azure.com)。
2. 选择“更多服务”，在文本框中输入 **Azure Active Directory**，然后选择“Enter”。
3. 在“Azure Active Directory -  ***目录名***”边栏选项卡（即，正在管理的目录的 Azure AD 边栏选项卡）中，选择“企业应用程序”。

    ![打开企业应用](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. 在“企业应用程序”边栏选项卡中，选择“所有应用程序”。 此时会显示可管理应用的列表。
5. 在在“企业应用程序 - 所有应用程序”边栏选项卡中，选择一个应用。
6. 在“appname”边栏选项卡（即标题中包含所选应用名称的边栏选项卡）中，选择“属性”。

    ![选择“所有应用程序”命令](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. 在“***appname*** - 属性”边栏选项卡中，对“启用以让用户登录?”设置选择“否”。
8. 选择“保存”命令。

## <a name="next-steps"></a>后续步骤
* [查看所有组](active-directory-groups-view-azure-portal.md)
* [向企业应用分配用户或组](active-directory-coreapps-assign-user-azure-portal.md)
* [删除企业应用的用户或组分配](active-directory-coreapps-remove-assignment-azure-portal.md)
* [更改企业应用的名称或徽标](active-directory-coreapps-change-app-logo-user-azure-portal.md)



<!--HONumber=Jan17_HO3-->


