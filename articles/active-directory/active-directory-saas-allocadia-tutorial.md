---
title: "教程：Azure Active Directory 与 Allocadia 集成 | Microsoft Docs"
description: "了解如何在 Azure Active Directory 和 Allocadia 之间配置单一登录。"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: c415fc55-6dc1-49f2-a8a2-2fc6e3790d65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 69473976eb0a9b6a0e9aa929581c0a7cf0c8d188
ms.openlocfilehash: e9d59c593c3637d72611516fd8a6b03f3f5f917b
ms.lasthandoff: 12/29/2016


---
# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a>教程：Azure Active Directory 与 Allocadia 集成
在本教程中，了解如何将 Allocadia 与 Azure Active Directory (Azure AD) 集成。

将 Allocadia 与 Azure AD 集成提供以下优势：

* 可在 Azure AD 中控制谁有权访问 Allocadia
* 可使用户通过其 Azure AD 帐户自动登录 Allocadia（单一登录）
* 可在一个中心位置（即 Azure 经典门户）管理帐户

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>先决条件
若要配置 Azure AD 与 Allocadia 的集成，需要以下项目：

* 一个 Azure AD 订阅
* 启用的订阅上的 Allocadia 单一登录

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。
> 
> 

测试本教程中的步骤应遵循以下建议：

* 不应使用生产环境，除非有此必要。
* 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Allocadia
2. 配置和测试 Azure AD 单一登录

## <a name="adding-allocadia-from-the-gallery"></a>从库中添加 Allocadia
若要配置 Allocadia 与 Azure AD 的集成，需要将库中的 Allocadia 添加到托管的 SaaS 应用列表。

**若要从库中添加 Allocadia，请执行以下步骤：**

1. 在 **Azure 经典门户**的左侧导航窗格上，单击“Active Directory”。 
   
    ![Active Directory][1]

2. 从“目录”列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。
   
    ![应用程序][2]

4. 在页面底部单击“添加”。
   
    ![应用程序][3]

5. 在“要执行什么操作”对话框中，单击“从库中添加应用程序”。
   
    ![应用程序][4]

6. 在“搜索框”中，键入“Allocadia”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_01.png)

7. 在“结果”窗格中，选择“Allocadia”，然后单击“完成”，添加该应用程序。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_06.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，根据名为“Britta Simon”的测试用户的指示配置和测试 Allocadia 的 Azure AD 单一登录。

对于单一登录到工作帐户，Azure AD 需要知道 Azure AD 用户在 Allocadia 中的对应用户是谁。 换句话说，需要建立 Azure AD 用户与 Allocadia 中相关用户之间的链接关系。
通过将 Azure AD 中的 **user name** 值分配为 Allocadia 中的 **Username** 值，建立此链接关系。

若要通过 Allocadia 配置和测试 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Allocadia 测试用户](#creating-an-allocadia-test-user)** - 在 Allocadia 中创建 Britta Simon 的对立用户，并且将其链接到她在 Azure AD 中的代表。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录
此部分的内容是在经典门户中启用 Azure AD 单一登录，并在 Allocadia 应用程序中配置单一登录。

Allocadia 应用程序需要采用特定格式的 SAML 断言。 请为此应用程序配置以下声明。 可从应用程序的“属性”选项卡管理这些属性的值。 以下屏幕截图显示一个示例。 

![配置单一登录](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_07.png) 

**若要配置 Hightail 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 经典门户的“Allocadia”应用程序集成页面上，单击顶部菜单中的“属性”。
   
    ![配置单一登录](./media/active-directory-saas-allocadia-tutorial/tutorial_general_80.png) 

2. 在“SAML 令牌属性”对话框中，对于下表中显示的每个行，执行以下步骤：
   
    | 属性名称 | 属性值 |
    | --- | --- |
    | 名 |user.givenname |
    | 姓 |user.surname |
    | 电子邮件 |user.mail |

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 单击“添加用户属性”，打开“添加用户属性”对话框。

    ![配置单一登录](./media/active-directory-saas-allocadia-tutorial/tutorial_general_81.png) 

    b. 在“属性名称”文本框中，键入为该行显示的属性名称。

    c. 在“属性值”列表中，选择为该行显示的属性值。

    d.单击“下一步”。 单击“完成”。    


1. 在顶部菜单中，单击“快速启动”。
   
    ![配置单一登录](./media/active-directory-saas-allocadia-tutorial/tutorial_general_83.png)  

2. 在“你希望用户如何登录 Allocadia”页上，选择“Azure AD 单一登录”，然后单击“下一步”。
   
    ![配置单一登录](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_03.png) 

3. 在“配置应用设置”对话框页上，执行以下步骤：
   
    ![配置单一登录](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_04.png) 
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在“标识符”框中，键入以下模式的 URL：对于测试环境，使用的 URL 模式如下：**“https://na2standby.allocadia.com”**，对于生产环境，使用**“https://na2.allocadia.com”**
   
    b. 在“回复 URL”中，键入以下模式的 URL：对于测试环境，使用的 URL 模式如下：**“https://na2standby.allocadia.com/allocadia/saml/SSO”**，对于生产环境，使用**“https://na2.allocadia.com/allocadia/saml/SSO”**

4. 在“配置 Allocadia 的单一登录”页面上，执行以下步骤：
   
    ![配置单一登录](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_05.png) 
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 单击“下载元数据”，然后在计算机上保存该文件。
   
    b. 单击“资源组名称” 的 Azure 数据工厂。

5. 若要为应用程序配置 SSO，请联系 [Allocadia 支持](mailTo:support@allocadia.com)团队，他们将协助配置 SSO。 请注意，需要发送电子邮件，并附件下载的元数据文件，以在 Allocadia 端配置 SSO。
   
    > [!NOTE]
    > 请确保 Allocadia 团队在测试环境中将标识符值设置为**“https://na2standby.allocadia.com”**，在生产环境中，该值应为：**“https://na2.allocadia.com”**
    > 
    > 

6. 在经典门户中，选择单一登录配置确认，然后单击“下一步”。
   
    ![Azure AD 单一登录][10]

7. 在“单一登录确认”页上，单击“完成”。  
   
    ![Azure AD 单一登录][11]

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
在本部分中，将在经典门户中创建一个名为 Britta Simon 的测试用户。
在“用户”列表中，选择“Britta Simon”。

![创建 Azure AD 用户][20]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 经典门户**中，在左侧导航窗格上，单击“Active Directory”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-allocadia-tutorial/create_aaduser_09.png) 

2. 在“目录”列表中，选择要启用目录集成的目录。

3. 若要显示用户列表，请在顶部菜单中，单击“用户”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. 若要打开“添加用户”对话框，请在底部工具栏中单击“添加用户”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

5. 在“告诉我们有关此用户的信息”对话框页上，执行以下步骤：
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-allocadia-tutorial/create_aaduser_05.png) 
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 对于“用户类型”，选择“组织中的新用户”。
   
    b. 在“用户名”文本框中，键入“BrittaSimon”。
   
    c. 单击“下一步”。

6. 在“用户配置文件”对话框页上，执行以下步骤：
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-allocadia-tutorial/create_aaduser_06.png) 
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在“名字”文本框中，键入“Britta”。  
   
    b. 在“姓氏”文本框中，键入“Simon”。
   
    c. 在“显示名称”文本框中，键入“Britta Simon”。
   
    d.单击“下一步”。 在“角色”列表中，选择“用户”。
   
    e.在“新建 MySQL 数据库”边栏选项卡中，接受法律条款，然后单击“确定”。 单击“下一步”。

7. 在“获取临时密码”对话框页上，单击“创建”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-allocadia-tutorial/create_aaduser_07.png) 

8. 在“获取临时密码”对话框页上，执行以下步骤：
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-allocadia-tutorial/create_aaduser_08.png) 
   
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 写下“新密码”的值。
   
    b. 单击“完成”。   

### <a name="creating-an-allocadia-test-user"></a>创建 Allocadia 测试用户
本部分的内容是在 Allocadia 中创建名为“Britta Simon”的用户。 Allocadia 应用程序支持准时用户预配。 如果已在上述**[配置 Azure AD 单一登录](#configuring-azure-ad-single-single-sign-on)**部分中配置了声明，则它将在应用程序中预配用户。 

> [!NOTE]
> 如果需要手动创建一个用户或一批用户，需要联系 Allocadia 支持团队。
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户
在本部分中，通过向 Britta Simon 授予 Allocadia 的访问权限支持她使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 Allocadia，请执行以下步骤：**

1. 在经典门户中，若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。
   
    ![分配用户][201] 

2. 在应用程序列表中，选择“Allocadia”。
   
    ![配置单一登录](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_50.png) 

3. 在顶部菜单中，单击“用户”。
   
    ![分配用户][203] 

4. 在“用户”列表中，选择“Britta Simon”。

5. 在底部工具栏中，单击“分配”。
   
    ![分配用户][205]

### <a name="testing-single-sign-on"></a>测试单一登录
在本部分中，使用访问面板测试 Azure AD 单一登录配置。
当在访问面板中单击“Allocadia”磁贴时，应该会自动登录“Allocadia”应用程序。

## <a name="additional-resources"></a>其他资源
* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](active-directory-saas-tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_205.png

