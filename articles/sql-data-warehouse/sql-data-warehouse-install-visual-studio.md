---
title: "安装适用于 SQL 数据仓库的 Visual Studio 和 SSDT | Microsoft Docs"
description: "安装适用于 Azure SQL 数据仓库的 Visual Studio 和 SQL Server 开发工具 (SSDT)"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 9c44f417d465aedcbbc731757282e18985b0435c


---
# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>安装适用于 SQL 数据仓库的 Visual Studio 2015 和 SSDT
若要开发 SQL 数据仓库的应用程序，建议使用 Visual Studio 2015，并结合最新版本的 SQL Server Data Tools (SSDT)。  向后兼容也支持装有 SSDT 的 Visual Studio 2013 Update 5。  

使用装有 SSDT 的 Visual Studio 让你可以使用 SQL Server 对象资源管理器以可视化方式浏览 SQL 数据仓库中的表格、视图、存储过程和其他更多对象，并运行查询。

> [!NOTE]
> SQL 数据仓库尚不支持 Visual Studio 数据库项目。  将来的版本会添加此功能。
> 
> 

## <a name="step-1-install-visual-studio-2015"></a>步骤 1：安装 Visual Studio 2015
遵循以下链接来下载并安装 Visual Studio 2015。 如果你已安装 Visual Studio 2013 或 2015，请跳到步骤 2 以安装 SSDT。

1. [下载 Visual Studio 2015][Download Visual Studio 2015]。
2. 按照 MSDN 上的 [安装 Visual Studio][Installing Visual Studio] 指南安装，并选择默认配置。

## <a name="step-2-install-ssdt"></a>步骤 2：安装 SSDT
若要安装适用于 Visual Studio 的 SSDT，只需遵循以下步骤从 Visual Studio 检查 SSDT 更新。

1. 在 Visual Studio 中，单击“**工具**” / “**扩展和更新...**” / “**更新**”
2. 选择“**产品更新**”，然后查找“**数据库工具的 Microsoft SQL Server 更新**”

如果找不到更新，则表示已安装最新版本。  若要确认是否已安装 SSDT，请单击“**帮助**” / “**关于 Microsoft Visual Studio**”，然后在列表中查找 SQL Server Data Tools。  SSDT 的最新版本是 14.0.60525.0。  如果 Visual Studio 中的安装选项不可用，你也可以访问 [SSDT 下载][SSDT Download]页面来手动下载并安装 SSDT。

## <a name="next-steps"></a>后续步骤
拥有最新版本的 SSDT 后，便可以[连接][connect]到你的 SQL 数据仓库。

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Download Visual Studio 2015]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx



<!--HONumber=Dec16_HO2-->


