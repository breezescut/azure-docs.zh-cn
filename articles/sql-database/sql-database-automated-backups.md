---
title: "Azure SQL 数据库备份 - 自动化、异地冗余 | Microsoft Docs"
description: "SQL 数据库每隔几分钟会自动创建一个本地数据库备份，使用 Azure 读取访问异地冗余存储来提供异地冗余。"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 3ee3d49d-16fa-47cf-a3ab-7b22aa491a8d
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2016
ms.author: sashan
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 8323aa27c93c1c460b31f7f7c822644f5eee929a
ms.lasthandoff: 03/28/2017


---
# <a name="learn-about-sql-database-backups"></a>了解 SQL 数据库备份

SQL 数据库会自动创建数据库备份，并使用 Azure 读取访问异地冗余存储 (RA-GRS) 来提供异地冗余。 可以自动创建这些备份且不收取额外费用。 无需执行任何操作即可创建这些备份。 数据库备份是任何业务连续性和灾难恢复策略的基本组成部分，因为数据库备份可以保护数据免遭意外损坏或删除。 如果想要在自己的存储容器中保存备份，可以配置长期备份保留策略。 有关详细信息，请参阅[长期保留](sql-database-long-term-retention.md)。

## <a name="what-is-a-sql-database-backup"></a>什么是 SQL 数据库备份？

SQL 数据库使用 SQL Server 技术创建[完整](https://msdn.microsoft.com/library/ms186289.aspx)、[差异](https://msdn.microsoft.com/library/ms175526.aspx)和[事务日志](https://msdn.microsoft.com/library/ms191429.aspx)备份。 一般每隔 5-10 分钟创建一次事务日志备份，具体频率取决于性能级别和数据库活动量。 借助事务日志备份以及完整备份和差异备份，可将数据库还原到托管数据库的服务器上的特定时间点。 还原数据库时，服务将会确定需要还原哪些完整、差异备份和事务日志备份。


可使用这些备份执行以下任务：

* 在保留期内将数据库还原到某个时间点。 此操作会在与原始数据库相同的服务器中创建新数据库。
* 将已删除的数据库还原到删除时的时间点或保留期内的任意时间点。 仅可在创建原始数据库所在的同一服务器中还原已删除的数据库。
* 将数据库还原到其他地理区域。 在无法访问服务器和数据库的情况下，此操作可帮助从地理位置灾难中恢复， 它会在全球任意位置的任意现有服务器中创建新数据库。 
* 从 Azure 恢复服务保管库中存储的特定备份还原数据库。 此操作允许还原旧版本的数据库，以满足符合性请求或运行旧版本的应用程序。 请参阅[长期保留](sql-database-long-term-retention.md)。
* 若要执行还原，请参阅[从备份还原数据库](sql-database-recovery-using-backups.md)。

> [!TIP]
> 有关教程，请参阅[数据保护和恢复的备份和还原入门](sql-database-get-started-backup-recovery-portal.md)
>


> [!NOTE]
> 在 Azure 存储中，术语*复制*是指将文件从一个位置复制到另一个位置。 SQL *数据库复制*是指让多个辅助数据库与主数据库保持同步。 
> 
> 

## <a name="how-much-backup-storage-is-included-at-no-cost"></a>免费附送的备份存储空间有多少？
SQL 数据库提供高达备份存储的最大预配数据库存储空间两倍的容量，不收取任何额外费用。 例如，如果你有一个标准数据库实例并且预配的数据库大小为 250 GB，则可以免费获得 500 GB 的备份存储空间。 如果数据库超过提供的备份存储空间，你可以选择与 Azure 支持联系来缩短保留期。 另一种做法是按照标准读取访问异地冗余存储 (RA-GRS) 费率付费购买额外的备份存储空间。 

## <a name="how-often-do-backups-happen"></a>备份频率如何？
每周执行一次完整数据库备份，通常每隔数小时执行一次差异数据库备份，通常每隔 5-10 分钟执行一次事务日志备份。 会在数据库创建后立即计划第一次完整备份。 完整备份通常可在 30 分钟内完成，但如果数据库很大，花费的时间可能更长。 例如，对已还原的数据库或数据库副本执行初始备份可能需要更长时间。 在完成首次完整备份后，将在后台以静默方式自动计划和管理所有后续备份。 在平衡整体系统工作负荷时，SQL 数据库服务会确定所有数据库备份的确切时间。 

备份存储异地复制根据 Azure 存储复制计划执行。

## <a name="how-long-do-you-keep-my-backups"></a>备份的保留时间有多长？
每个 SQL 数据库备份都有一个保留期，该期限基于数据库的[服务层](sql-database-service-tiers.md)。 各服务层中数据库的保留期如下：


* 基本服务层为 7 天。
* 标准服务层为 35 天。
* 高级服务层为 35 天。

如果将数据库从标准或高级服务层降级到基本服务层，备份将保存 7 天。 超过 7 天的所有现有备份将被清除。 

如果将数据库从基本服务层升级到标准或高级服务层，SQL 数据库将保存现有备份，直到超过 35 天。 创建的新备份将保存 35 天。

如果删除了某个数据库，SQL 数据库将保存其备份，就像保存联机数据库的备份一样。 例如，假设你删除了保留期为 7 天的某个基本数据库。 已保存 4 天的备份将继续保存 3 天。

> [!IMPORTANT]
> 如果删除了托管 SQL 数据库的 Azure SQL 服务器，则属于该服务器的所有数据库也将被删除且不可恢复。 无法还原已删除的服务器。
> 
> 

## <a name="how-to-extend-the-backup-retention-period"></a>如何延长备份保留期？
如果应用程序要求备份保留更长时间，可通过为每个数据库配置长期备份保留策略（LTR 策略）来延长内置保留期。 允许将内置保持期从 35 天延长为最多 10 年。 有关详细信息，请参阅[长期保留](sql-database-long-term-retention.md)。

将 LTR 策略添加到使用 Azure 门户或 API 的数据库后，每周的完整数据库备份会自动复制到自己的 Azure 备份服务保管库。 如果使用 TDE 加密数据库，备份会在静止时自动加密。  服务保管库会根据时间戳和 LTR 策略自动删除过期的备份。  因此，无需管理备份计划，也不用担心旧文件的清除工作。 只要保管库与 SQL 数据库位于同一订阅中，还原 API 就支持存储在保管库中的备份。 可以使用 Aure 门户或 PowerShell 访问这些备份。

> [!TIP]
> 有关教程，请参阅[数据保护和恢复的备份和还原入门](sql-database-get-started-backup-recovery-portal.md)
>

## <a name="next-steps"></a>后续步骤

- 数据库备份是任何业务连续性和灾难恢复策略的基本组成部分，因为数据库备份可以保护数据免遭意外损坏或删除。 若要了解其他 Azure SQL 数据库业务连续性解决方案，请参阅[业务连续性概述](sql-database-business-continuity.md)。
- 若要使用 Azure 门户还原到某个时间点，请参阅[使用 Azure 门户将数据库还原到某个时间点](sql-database-point-in-time-restore-portal.md)。
- 若要使用 PowerShell 还原到某个时间点，请参阅[使用 PowerShell 将数据库还原到某个时间点](scripts/sql-database-restore-database-powershell.md)。
- 若要使用 Azure 门户在 Azure 恢复服务保管库中配置和管理自动备份的长期保留，并从中进行还原，请参阅[使用 Azure 门户管理长期备份保留](sql-database-manage-long-term-backup-retention-portal.md)。
- 若要使用 PowerShell 在 Azure 恢复服务保管库中配置和管理自动备份的长期保留，并从中进行还原，请参阅[使用 PowerShell 管理长期备份保留](sql-database-manage-long-term-backup-retention-powershell.md)。

