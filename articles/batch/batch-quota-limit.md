<properties
	pageTitle="Batch 服务配额和限制 | Microsoft Azure"
	description="了解使用 Azure Batch 服务时的配额、限制和约束"
	services="batch"
	documentationCenter=""
	authors="dlepow"
	manager="timlt"
	editor=""/>

<tags
	ms.service="batch"
	ms.date="10/26/2015"
	wacn.date=""/>



# Azure Batch 服务的配额和限制

本文列出可在 Azure Batch 服务中使用的特定资源的默认值和最大限制。其中的大多数限制都是 Azure 应用于订阅或 Batch 帐户的配额。

如果你打算运行生产 Batch 工作负荷，则可能需要增加一个或多个高于默认值的配额。如果需要提高配额，可以免费提出在线客户支持请求。

>[AZURE.NOTE]配额是一种信用限制，不附带容量保证。如果你有大规模的容量需求，请联系 Azure 支持。

## 订阅配额
资源|默认限制|最大限制
---|---|---
每个区域每个订阅的 Batch 帐户数|1|50

## Batch 帐户配额
[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

## 其他限制
资源|最大限制
---|---
每个计算节点的任务数|4 x 节点核心数

## 查看 Batch 配额

可在 [Azure 门户](https://manage.windowsazure.cn)中查看 Batch 帐户配额。

1. 在管理门户中，单击“Batch 帐户”，然后单击你的 Batch 帐户的名称。

2. 在帐户边栏选项卡中，单击“设置”>“属性”。

	![Batch 帐户配额][account_quotas]

3. 在“属性”边栏选项卡中，检查当前应用于 Batch 帐户的配额。

## 增加配额

在 Azure 门户中使用以下步骤请求增加配额（也可以在 [Azure 经典门户](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)中请求增加）。

1. 在预览门户的仪表板中，单击“帮助 + 支持”。

2. 单击“创建支持请求”>“基本”。

3. 在“基本”边栏选项卡中执行以下操作：

	a.在“问题类型”中，选择“配额”。

	b.选择你的订阅。

	c.在“服务”中，选择“Batch 服务”。

	d.在“支持计划”中，选择“Azure 支持计划 - 开发人员”。

	单击**“下一步”**。

4. 在“问题”边栏选项卡中执行以下操作：

	a.在“问题类型”中，选择“Batch”。

	b.在“详细信息”中，列出要在特定帐户中更改的配额和所需的新限制。

	单击**“下一步”**。

5. 在“联系信息”边栏选项卡中输入你的详细联系信息，然后单击“下一步”。

6. 单击“创建”提交新的支持请求。

Azure 支持人员将与你联系。完成请求最长需要 2 个工作日。

## 相关主题

* [创建和管理 Azure Batch 帐户](batch-account-create-portal.md)

* [Azure Batch 功能概述](batch-api-basics.md)

* [Azure 订阅和服务限制、配额和约束](../azure-subscription-service-limits.md)

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG

<!---HONumber=Mooncake_0118_2016-->