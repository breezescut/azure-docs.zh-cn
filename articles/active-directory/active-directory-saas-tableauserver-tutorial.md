---
title: "教程：Azure Active Directory 与 Tableau Server 集成 | Microsoft 文档"
description: "了解如何在 Azure Active Directory 和 Tableau Server 之间配置单一登录。"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 0849c77c66ac8617e217a69696b5c404be3b5eb4
ms.openlocfilehash: 3aefb8188880fac5c1fcbe237a1e133584089e6a

---

# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>教程：Azure Active Directory 与 Tableau Server 集成
本教程的目的是说明如何将 Tableau Server 与 Azure Active Directory (Azure AD) 集成。

将 Tableau Server 与 Azure AD 集成提供以下优势：

* 可在 Azure AD 中控制谁有权访问 Tableau Server
* 可以让用户使用其 Azure AD 帐户自动登录到 Tableau Server（单一登录）
* 可使用 Azure 经典门户在一个中心位置管理帐户

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>先决条件
若要配置 Azure AD 与 Tableau Server 的集成，需要以下项：

* Azure AD 订阅
* 已启用 Tableau Server 单一登录 (SSO) 的订阅

> [!NOTE]
> 测试本教程中的步骤时，建议不要使用生产环境。
> 

测试本教程中的步骤应遵循以下建议：

* 不应使用生产环境，除非有此必要。
* 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
本教程的目的是介绍如何在测试环境中测试 Azure AD 单一登录。 

本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Tableau Server
2. 配置和测试 Azure AD 单一登录

## <a name="adding-tableau-server-from-the-gallery"></a>从库中添加 Tableau Server
若要配置 Tableau Server 与 Azure AD 的集成，需要从库中将 Tableau Server 添加到托管 SaaS 应用列表。

**若要从库中添加 Tableau Server，请执行以下步骤：**

1. 在 **Azure 经典门户**的左侧导航窗格上，单击“Active Directory”。 
   
    ![Active Directory][1]
2. 从“目录”列表中，选择要为其启用目录集成的目录。
3. 若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。
   
    ![应用程序][2]
4. 在页面底部单击“添加”。
   
    ![应用程序][3]
5. 在“要执行什么操作”对话框中，单击“从库中添加应用程序”。
   
    ![应用程序][4]
6. 在搜索框中，键入“Tableau Server”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)
7. 在结果窗格中，选择“Tableau Server”，然后单击“完成”以添加该应用程序。
   
    ![在库中选择应用](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是说明如何基于名为“Britta Simon”的测试用户配置和测试 Tableau Server 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Tableau Server 用户。 换句话说，需要在 Azure AD 用户与 Tableau Server 中相关用户之间建立链接关系。

通过将 Azure AD 中“用户名”的值分配为 Tableau Server 中“用户名”的值来建立此链接关系。

若要配置和测试 Tableau Server 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Tableau Server 测试用户](#creating-a-tableauserver-test-user)** - 在 Tableau Server 中创建 Britta Simon 的对应用户，将其链接到她的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录
本部分的目的是在 Azure 经典门户中启用 Azure AD 单一登录并在 Tableau Server 应用程序中配置单一登录。

Tableau Server 应用程序需要采用特定格式的 SAML 断言。 以下屏幕截图显示一个示例。 

![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**若要配置 Tableau Server 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 经典门户的“Tableau Server”应用程序集成页上，单击顶部菜单中的“属性”。
   
    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 
2. 在“SAML 令牌属性”对话框中，执行以下步骤：

   1. 单击“添加用户属性”，打开“添加用户属性”对话框。

    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 
   2. 在“属性名称”文本框中，键入“username”。
   3. 从“属性值”列表中，选择“user.displayname”。
   4. 单击“完成”。    

3. 在顶部菜单中，单击“快速启动”。
   
    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  
4. 单击“配置单一登录”，打开“配置单一登录”对话框。
   
    ![配置单一登录][6] 
5. 在“你希望用户如何登录 Tableau Server”页上，选择“Azure AD 单一登录”，然后单击“下一步”。
   
    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 
6. 在“配置应用设置”对话框页上，执行以下步骤，然后单击“下一步”：
   
    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 

   1. 在“登录 URL”文本框中，键入 Tableau Server 的 URL。 
   2. 在“标识符”框中，复制 URL。
   3. 单击“下一步”。
h
7. 在“配置 Tableau Server 的单一登录”页上，执行以下步骤并单击“下一步”：
   
    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 

   1. 单击“下载元数据”，然后在计算机上保存该文件。
   2. 单击“下一步”。

8. 若要为应用程序配置 SSO，需要以管理员身份登录 Tableau Server 租户。
   
   1. 在 Tableau Server 配置中，单击“SAML”选项卡。
  
    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
   2. 选中“使用 SAML 进行单一登录”复选框。
   3. 找到从 Azure 经典门户下载的联合元数据文件，然后在“SAML Idp 元数据文件”中上载它。
   4. Tableau Server 返回 URL（Tableau Server 用户将要访问的 URL），例如 http://tableau_server。 不建议使用 http://localhost。 不支持使用带尾部反斜杠的 URL（例如，http://tableau_server/）。 复制 **Tableau Server 返回 URL** 并将其粘贴到 Azure AD“登录 URL”文本框中，如步骤 3 中所示
   5. SAML 实体 ID - 此实体 ID 唯一标识安装到 IdP 的 Tableau Server。 可以在此处再次输入 Tableau Server URL（如果需要），但它不必须是 Tableau Server URL。 复制 **SAML 实体 ID** 并将其粘贴到 Azure AD“标识符”文本框中，如步骤 3 中所示。
   6. 单击“导出元数据文件”，并在文本编辑器应用程序中打开该文件。 找到包含 Http Post 和索引 0 的断言使用者服务 URL 并复制该 URL。 现在将其粘贴到 Azure AD“回复 URL”文本框中，如步骤 3 中所示。 
   7. 在“Tableau Server 配置”页中单击“确定”按钮。
   
    >[!NOTE] 
    >如果需要帮助在 Tableau Server 上配置 SAML，请参阅此文：[配置 SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm)。
    >

9. 在 Azure 经典门户中，选择“单一登录配置确认”，然后单击“下一步”。
   
    ![Azure AD 单一登录][10]
10. 在“单一登录确认”页上，单击“完成”。 
   
    ![Azure AD 单一登录][11]

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 经典门户中创建名为“Britta Simon”的测试用户。

* 在“用户列表”中，选择“Britta Simon”。

![创建 Azure AD 用户][20]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 经典门户**中，在左侧导航窗格上，单击“Active Directory”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 
2. 在“目录”列表中，选择要启用目录集成的目录。
3. 若要显示用户列表，请在顶部菜单中，单击“用户”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 
4. 若要打开“添加用户”对话框，请在底部工具栏中单击“添加用户”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)
5. 在“告诉我们有关此用户的信息”对话框页上，执行以下步骤：
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 
   
   1. 对于“用户类型”，选择“组织中的新用户”。
   2. 在“用户名”文本框中，键入“BrittaSimon”。
   3. 单击“下一步”。
6. 在“用户配置文件”对话框页上，执行以下步骤：
   
   ![创建 Azure AD 测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 
   
   1. 在“名字”文本框中，键入“Britta”。  
   2. 在“姓氏”文本框中，键入“Simon”。
   3. 在“显示名称”文本框中，键入“Britta Simon”。
   4. 在“角色”列表中，选择“用户”。
   5. 单击“下一步”。
7. 在“获取临时密码”对话框页上，单击“创建”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 
8. 在“获取临时密码”对话框页上，执行以下步骤：
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 
   
   1. 写下“新密码”的值。
   2. 单击“完成”。   

### <a name="creating-a-tableau-server-test-user"></a>创建 Tableau Server 测试用户
本部分的目的是在 Tableau Server 中创建名为“Britta Simon”的用户。 需要在 Tableau Server 中预配所有用户。 另请注意，用户的用户名应与在 Azure AD 自定义属性 **username** 中配置的值匹配。 使用正确的映射，此集成应可实现[配置 Azure AD 单一登录](#configuring-azure-ad-single-single-sign-on)。

> [!NOTE]
> 如果需要手动创建用户，需要联系组织中的 Tableau Server 管理员。
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户
本部分的目的是通过授予 Britta Simon 访问 Tableau Server 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 Tableau Server，请执行以下步骤：**

1. 在 Azure 经典门户中，若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。
   
    ![分配用户][201] 
2. 在应用程序列表中，选择“Tableau Server”。
   
    ![配置单一登录](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 
3. 在顶部菜单中，单击“用户”。
   
    ![分配用户][203]
4. 在“用户”列表中，选择“Britta Simon”。
5. 在底部工具栏中，单击“分配”。

![分配用户][205]

### <a name="testing-single-sign-on"></a>测试单一登录
本部分的目的是使用“访问面板”测试 Azure AD 单一登录配置。

单击访问面板中的“Tableau Server”磁贴时，用户应自动登录到 Tableau Server 应用程序。

## <a name="additional-resources"></a>其他资源
* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](active-directory-saas-tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png



<!--HONumber=Feb17_HO1-->

