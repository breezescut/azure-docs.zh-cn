---
title: "教程：Azure Active Directory 与 Pantheon 集成 | Microsoft 文档"
description: "了解如何在 Azure Active Directory 和 Pantheon 之间配置单一登录。"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 52a42d6705d0d2cfb86865027f8315411d7aece0
ms.openlocfilehash: b14872daab1bddf18d68b1f0fb23fc74c12078ed


---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a>教程：Azure Active Directory 与 Pantheon 集成

在本教程中，了解如何将 Pantheon 与 Azure Active Directory (Azure AD) 集成。

将 Pantheon 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Pantheon
- 可以让用户通过其 Azure AD 帐户自动登录到 Pantheon（单一登录）
- 可在一个中心位置（即 Azure 经典门户）管理帐户

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Pantheon 的集成，需要以下项：

- Azure AD 订阅
- 已启用 Pantheon 单一登录的订阅


>[!NOTE] 
>不建议使用生产环境测试本教程中的步骤。


测试本教程中的步骤应遵循以下建议：

- 不应使用生产环境，除非有此必要。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。


## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。

本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Pantheon
2. 配置和测试 Azure AD 单一登录


## <a name="adding-pantheon-from-the-gallery"></a>从库中添加 Pantheon
若要配置 Pantheon 与 Azure AD 的集成，需要从库中将 Pantheon 添加到托管 SaaS 应用列表。

**若要从库中添加 Pantheon，请执行以下步骤：**

1. 在 **Azure 经典门户**的左侧导航窗格上，单击“Active Directory”。

    ![Active Directory][1]

2. 从“目录”列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。

    ![应用程序][2]

4. 在页面底部单击“添加”。

    ![应用程序][3]

5. 在“要执行什么操作”对话框中，单击“从库中添加应用程序”。

    ![应用程序][4]

6. 在搜索框中，键入“Pantheon”。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_01.png)

7. 在结果窗格中，选择“Pantheon”，然后单击“完成”以添加该应用程序。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 Pantheon 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Pantheon 用户。 换句话说，需要在 Azure AD 用户与 Pantheon 中相关用户之间建立链接关系。

通过将 Azure AD 中“用户名”的值分配为 Pantheon 中“用户名”的值来建立此链接关系。

若要配置并测试 Pantheon 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Pantheon 测试用户](#creating-a-pantheon-test-user)** - 在 Pantheon 中创建 Britta Simon 的对应用户，将其链接到她的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，在经典门户中启用 Azure AD 单一登录，并在 Pantheon 应用程序中配置单一登录。


**若要配置 Pantheon 的 Azure AD 单一登录，请执行以下步骤：**

Pantheon 应用程序需要特定格式的 SAML 断言，这就需要使用用户的电子邮件地址设置 NameIdentifier 属性值。 默认情况下，Azure AD 将 UserPrincipalName 用于 NameIdentifier 属性。 但为了成功集成，需要将此值调整为与用户的电子邮件地址匹配。
只有在完成相应的映射后，集成才有效。

![配置单一登录](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_06.png)
    
1. 在经典门户中的“Pantheon”应用程序集成页上，单击“配置单一登录”，打开“配置单一登录”对话框。
     
    ![配置单一登录][6] 

2. 在“你希望用户如何登录 Pantheon”页上，选择“Azure AD 单一登录”，然后单击“下一步”。

    ![配置单一登录](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_03.png) 

3. 在“配置应用设置”对话框页上，执行以下步骤：

    ![配置单一登录](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_04.png) 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在“标识符”文本框中，使用以下模式键入 URN：`urn:auth0:pantheon:<orgname>-SSO`
    
    b.保留“数据库类型”设置，即设置为“共享”。 在“回复 URL”文本框中，使用以下模式键入 URL：`https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO` 

    c. 单击“下一步”
 
4. 在“配置 Pantheon 的单一登录”页上，执行以下步骤：

    ![配置单一登录](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_05.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 单击“下载证书”，然后将文件保存在计算机上。

    b. 单击“下一步”。


5. 若要为应用程序配置 SSO，请联系 [Pantheon 支持](https://pantheon.io/docs/getting-support)团队，并向其提供以下信息：

    • 下载的 **证书**

    • **单一登录服务 URL**

    >[!NOTE] 
    >还需要提供电子邮件域信息和要启用此连接的日期时间。 可以从[此处](https://pantheon.io/docs/sso-organizations/)找到更多详细信息

6. 在经典门户中，选择“单一登录配置确认”，然后单击“下一步”。
    
    ![Azure AD 单一登录][10]

7. 在“单一登录确认”页上，单击“完成”。  
 
    ![Azure AD 单一登录][11]


### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
在本部分中，在经典门户中创建名为“Britta Simon”的测试用户。


![创建 Azure AD 用户][20]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 经典门户**中，在左侧导航窗格上，单击“Active Directory”。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-pantheon-tutorial/create_aaduser_09.png) 

2. 在“目录”列表中，选择要启用目录集成的目录。

3. 若要显示用户列表，请在顶部菜单中，单击“用户”。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-pantheon-tutorial/create_aaduser_03.png) 

4. 若要打开“添加用户”对话框，请在底部工具栏中单击“添加用户”。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-pantheon-tutorial/create_aaduser_04.png) 

5. 在“告诉我们有关此用户的信息”对话框页中，执行以下步骤： ![创建 Azure AD 测试用户](./media/active-directory-saas-pantheon-tutorial/create_aaduser_05.png) 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 对于“用户类型”，选择“组织中的新用户”。

    b. 在“用户名”文本框中，键入“BrittaSimon”。

    c. 单击“资源组名称” 的 Azure 数据工厂。

6.  在“用户配置文件”对话框页面上，执行以下步骤：![创建 Azure AD 测试用户](./media/active-directory-saas-pantheon-tutorial/create_aaduser_06.png) 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在“名字”文本框中，键入“Britta”。  

    b. 在“姓氏”文本框中，键入“Simon”。

    c. 在“显示名称”文本框中，键入“Britta Simon”。

    d.单击“下一步”。 在“角色”列表中，选择“用户”。

    e.在“新建 MySQL 数据库”边栏选项卡中，接受法律条款，然后单击“确定”。 单击“下一步”。

7. 在“获取临时密码”对话框页上，单击“创建”。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-pantheon-tutorial/create_aaduser_07.png) 

8. 在“获取临时密码”对话框页上，执行以下步骤：

    ![创建 Azure AD 测试用户](./media/active-directory-saas-pantheon-tutorial/create_aaduser_08.png) 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 写下“新密码”的值。

    b. 单击“完成”。   



### <a name="creating-an-pantheon-test-user"></a>创建 Pantheon 测试用户

在本部分中，将在 Pantheon 中创建一个名为“Britta Simon”的用户。 请按照以下步骤将用户添加到 Pantheon 中。 

>[!NOTE] 
>要使 SSO 正常工作，需要先在 Pantheon 中创建用户。

1. 使用管理员凭据登录到 Pantheon。

2. 导航到“组织”仪表板页。
 
3. 单击“人员”。

4. 单击“添加用户”。

5. 输入用户的电子邮件地址。

6. 选择用户的角色。

7. 单击“添加用户”。


### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Pantheon 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 Pantheon，请执行以下步骤：**

1. 在经典门户中，若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。

    ![分配用户][201] 

2. 在应用程序列表中，选择“Pantheon”。

    ![配置单一登录](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_50.png) 

3. 在顶部菜单中，单击“用户”。

    ![分配用户][203]

4. 在“用户”列表中，选择“Britta Simon”。

5. 在底部工具栏中，单击“分配”。

    ![分配用户][205]


### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的“Pantheon”磁贴时，用户应自动登录到 Pantheon 应用程序。


## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](active-directory-saas-tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_205.png



<!--HONumber=Dec16_HO1-->


