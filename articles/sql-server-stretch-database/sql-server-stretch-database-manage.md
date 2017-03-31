---
title: "Stretch Database 的管理和故障排除 | Microsoft 文档"
description: "如何对 Stretch Database 进行管理和故障排除。"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 8a43c0fc-64d3-4042-8921-a36542aa8933
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2016
ms.author: douglasl
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 681ad472e53a17600b589d8354d9fdb5c9c3c574


---
# <a name="manage-and-troubleshoot-stretch-database"></a>Stretch Database 的管理和故障排除
若要对 Stretch Database 进行管理和故障排除，请使用本主题中所述的工具和方法。

## <a name="manage-local-data"></a>管理本地数据
### <a name="a-namelocalinfoaget-info-about-local-databases-and-tables-enabled-for-stretch-database"></a><a name="LocalInfo"></a>获取有关已启用 Stretch Database 的本地数据库和表的信息
打开目录视图 **sys.databases** 和 **sys.tables**，以查看有关已启用延伸的 SQL Server 数据库和表的信息。 有关详细信息，请参阅 [sys.databases (Transact-SQL)](https://msdn.microsoft.com/library/ms178534.aspx) 和 [sys.tables (Transact-SQL)](https://msdn.microsoft.com/library/ms187406.aspx)。

若要查看已启用延伸的表在 SQL Server 中使用的空间量，请运行以下语句。

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>管理数据迁移
### <a name="check-the-filter-function-applied-to-a-table"></a>检查应用于表的筛选器函数
打开目录视图 **sys.remote\_data\_archive\_tables**，并检查 **filter\_predicate** 列的值，以标识 Stretch Database 正在使用来选择要迁移的行的函数。 如果值为 null，则整个表符合迁移条件。 有关详细信息，请参阅 [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx) 和[使用筛选器函数选择要迁移的行](sql-server-stretch-database-predicate-function.md)。

### <a name="a-namemigrationacheck-the-status-of-data-migration"></a><a name="Migration"></a>检查数据迁移状态
在 SQL Server Management Studio 中选择数据库对应的“**任务 | 延伸 | 监视**”，以便在 Stretch Database 监视器中监视数据迁移。 有关详细信息，请参阅[数据迁移的监视和故障排除 (Stretch Database)](sql-server-stretch-database-monitor.md)。

或者，打开动态管理视图 **sys.dm\_db\_rda\_migration\_status** 查看已迁移的数据批数与行数。

### <a name="a-namefirewallatroubleshoot-data-migration"></a><a name="Firewall"></a>数据迁移的故障排除
有关故障排除的建议，请参阅[数据迁移的监视和故障排除 (Stretch Database)](sql-server-stretch-database-monitor.md)。

## <a name="manage-remote-data"></a>管理远程数据
### <a name="a-nameremoteinfoaget-info-about-remote-databases-and-tables-used-by-stretch-database"></a><a name="RemoteInfo"></a>获取有关 Stretch Database 使用的远程数据库和表的信息
打开目录视图 **sys.remote\_data\_archive\_databases** 和 **sys.remote\_data\_archive\_tables**，以查看有关存储迁移数据的远程数据库和表的信息。 有关详细信息，请参阅 [sys.remote_data_archive_databases (Transact-SQL)](https://msdn.microsoft.com/library/dn934995.aspx) 和 [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx)。

若要查看已启用延伸的表在 Azure 中使用的空间量，请运行以下语句。

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>删除迁移的数据
如果你想删除已经迁移到 Azure 的数据，请按照 [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx) 中所述步骤进行操作。

## <a name="manage-table-schema"></a>管理表架构
### <a name="dont-change-the-schema-of-the-remote-table"></a>不要更改远程表的架构
不要更改与针对 Stretch Database 配置的 SQL Server 表相关联的远程 Azure 表的架构。 具体而言，请不要修改列的名称或数据类型。 Stretch Database 功能会在与 SQL Server 表架构关联的远程表架构方面做出多种假设。 如果你更改了远程架构，所更改表的 Stretch Database 将停止工作。

### <a name="reconcile-table-columns"></a>协调表列
如果你意外地从远程表中删除了列，可运行 **sp_rda_reconcile_columns** 将列添加到远程表中，这些列在启用了延伸的 SQL Server 表中存在但在远程表中不存在。 有关详细信息，请参阅 [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx)。  

> [!IMPORTANT]
> 当 **sp_rda_reconcile_columns** 重新创建你意外从远程表中删除的列时，它不会还原之前存在于已删除列中的数据。
> 
> 

**sp_rda_reconcile_columns** 不会从远程表中删除在远程表中存在但在启用了延伸的 SQL Server 表中不存在的列。 如果远程 Azure 表中具有不再存在于启用了延伸的 SQL Server 表的列，这些额外的列也不会妨碍 Stretch Database 的正常运作。 你可以选择手动删除额外的列。  

## <a name="manage-performance-and-costs"></a>管理性能和成本
### <a name="troubleshoot-query-performance"></a>查询性能故障排除
包含已启用延伸的表的查询在执行时的速度预期比启用延伸之前要慢。 如果查询性能显著下降，请查看以下可能的问题。

* 你的 SQL Server 所在的地理区域是否与 Azure 服务器不同？ 在与 SQL Server 相同的地理区域配置 Azure 服务器可以降低网络延迟。
* 你的网络条件可能有所退化。 有关最近出现的问题或服务中断的信息，请与你的网络管理员联系。

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>提高 Azure 性能级别，以便能够执行索引编制等资源密集型操作
在生成、重新生成或重新组织已针对 Stretch Database 配置的大型表中的索引时，你期望在此期间在 Azure 中对迁移的数据进行大量的查询，那么请考虑在该操作的持续时间内提高相应远程 Azure 数据库的性能级别。 有关性能级别和定价的详细信息，请参阅 [SQL Server Stretch Database 定价](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)。

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>你无法暂停 Azure 上的 SQL Server Stretch Database 服务
 请确保你选择合适的性能和定价级别。 如果你为某个资源密集型操作暂时增加性能级别，那么在操作完成后，请将它还原到之前的级别。 有关性能级别和定价的详细信息，请参阅 [SQL Server Stretch Database 定价](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)。  

## <a name="change-the-scope-of-queries"></a>更改查询的范围
 默认情况下，针对启用了延伸的表进行的查询将返回本地和远程数据。 你可以更改所有用户进行的所有查询的范围，或仅更改管理员进行的单个查询的范围。  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>更改所有用户进行的所有查询的范围
 若要更改所有用户进行的所有查询的范围，请运行存储过程 **sys.sp_rda_set_query_mode**。 你可以将范围缩小到仅查询本地数据、禁用所有查询，或还原默认设置。 有关详细信息，请参阅 [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx)。  

### <a name="a-namequeryhintsachange-the-scope-of-queries-for-a-single-query-by-an-administrator"></a><a name="queryHints"></a>更改管理员进行的单个查询的查询范围
 若要更改 db_owner 角色的成员进行的单个查询的范围，请将 **WITH \( REMOTE_DATA_ARCHIVE_OVERRIDE = *value* \)** 查询提示添加到 SELECT 语句。 REMOTE_DATA_ARCHIVE_OVERRIDE 查询提示可具有以下值。  

* **LOCAL_ONLY**。 仅查询本地数据。  
* **REMOTE_ONLY**。 仅查询远程数据。  
* **STAGE_ONLY**。 仅查询表中的数据，在该表中，Stretch Database 暂存可进行迁移的行并在迁移后的指定时期内保留已迁移的行。 此查询提示是查询临时表的唯一方法。  

例如，以下查询仅返回本地结果。  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="a-nameadminhintsamake-administrative-updates-and-deletes"></a><a name="adminHints"></a>进行管理更新和删除操作
 默认情况下，在启用了延伸的表中不能“更新”或“删除”可进行迁移的行或已迁移的行。 当你必须解决问题时，db_owner 角色的成员可以通过将 **WITH \( REMOTE_DATA_ARCHIVE_OVERRIDE = *value* \)** 查询提示添加到语句来运行“更新”或“删除”操作。 REMOTE_DATA_ARCHIVE_OVERRIDE 查询提示可具有以下值。  

* **LOCAL_ONLY**。 仅更新或删除本地数据。  
* **REMOTE_ONLY**。 仅更新或删除远程数据。  
* **STAGE_ONLY**。 仅更新或删除表中的数据，在该表中，Stretch Database 暂存可进行迁移的行并在迁移后的指定时期内保留已迁移的行。  

## <a name="see-also"></a>另请参阅
[监视 Stretch Database](sql-server-stretch-database-monitor.md)

[备份启用了延伸的数据库](sql-server-stretch-database-backup.md)

[还原已启用延伸的数据库](sql-server-stretch-database-restore.md)




<!--HONumber=Nov16_HO3-->

