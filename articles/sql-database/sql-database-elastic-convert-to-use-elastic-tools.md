---
title: "迁移要扩展的现有数据库 | Microsoft 文档"
description: "通过创建分片映射管理器来转换分片数据库，以使用弹性数据库工具"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 8c851d8e-8fd5-4327-89c1-9178b20ddd69
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 10/24/2016
ms.author: ddove
translationtype: Human Translation
ms.sourcegitcommit: 10b40214ad4c7d7bb7999a5abce1c22100b617d8
ms.openlocfilehash: 6f51f0585dfc75d28da72d1056ef7173c06b24fd
ms.lasthandoff: 01/13/2017


---
# <a name="migrate-existing-databases-to-scale-out"></a>迁移要扩展的现有数据库
使用 Azure SQL 数据库数据库工具（例如[弹性数据库客户端库](sql-database-elastic-database-client-library.md)）轻松管理现有的扩展共享数据库。 必须先转换现有数据库集，以使用[分片映射管理器](sql-database-elastic-scale-shard-map-management.md)。 

## <a name="overview"></a>概述
迁移现有分片数据库： 

1. [准备分片映射管理器数据库](sql-database-elastic-scale-shard-map-management.md)。
2. 创建分片映射。
3. 准备各个分片。  
4. 将映射添加到分片映射。

你可以使用 [.NET Framework 客户端库](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)或者 [Azure SQL DB - 弹性数据库工具脚本](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)中提供的 PowerShell 脚本来实现这些技巧。 以下示例使用 PowerShell 脚本。

有关 ShardMapManager 的详细信息，请参阅[分片映射管理](sql-database-elastic-scale-shard-map-management.md)。 有关弹性数据库工具的概述，请参[阅弹性数据库功能概述](sql-database-elastic-scale-introduction.md)。

## <a name="prepare-the-shard-map-manager-database"></a>准备分片映射管理器数据库
分片映射管理器是一个特殊数据库，其中包含用来管理已扩展数据库的数据。 你可使用现有数据库或创建新数据库。 请注意，用作分片映射管理器的数据库不应是与分片相同的数据库。 另请注意：PowerShell 脚本不会为你创建该数据库。 

## <a name="step-1-create-a-shard-map-manager"></a>步骤 1：创建分片映射管理器
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>检索分片映射管理器
创建后，可以使用此 cmdlet 检索分片映射管理器。 每当需要使用 ShardMapManager 对象时，就需要执行此步骤。

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-the-shard-map"></a>第 2 步：创建分片映射
必须选择要创建的分片映射类型。 选择取决于数据库架构： 

1. 每个数据库一个租户（有关术语，请参阅[词汇表](sql-database-elastic-scale-glossary.md)。） 
2. 每个数据库多个租户（两种类型）：
   1. 列表映射
   2. 范围映射

对于单租户模型，创建**列表映射**分片映射。 单租户模型将每个租户分配一个数据库。 这是适用于 SaaS 开发人员的有效模式，因为它可以简化管理。

![列表映射][1]

多租户模型将数个租户分配给单一数据库（你可以跨多个数据库分布租户组。） 当希望每个租户具有较小数据需求时使用此模型。 在此模型中，我们使用**范围映射**将一系列用户分配到数据库。 

![范围映射][2]

或者你可以使用*列表映射*来实现多租户数据库模型，以将多个租户分配给单一数据库。 例如，DB1 用于存储租户 ID 1 和 5 的相关信息，而 DB2 用于存储租户 7 和租户 10 的数据。 

![单一数据库上的多个租户][3] 

**根据你的选择，选择以下选项之一：**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>选项 1：为列表映射创建分片映射
使用 ShardMapManager 对象创建分片映射。 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>选项 2：为范围映射创建分片映射
请注意，若要使用此映射模式，租户 ID 值需是连续范围，并且可接受范围中有间距，方法为只在创建数据库时跳过范围。

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>选项 3：单一数据库上的列表映射
设置此模式也需要创建列表映射，如步骤 2，选项 1 中所示。

## <a name="step-3-prepare-individual-shards"></a>步骤 3：准备各个分片
将每个分片（数据库）添加到分片映射管理器。 这将准备用于存储映射信息的各个数据库。 对每个分片执行此方法。

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.


## <a name="step-4-add-mappings"></a>步骤 4：添加映射
添加映射的操作取决于创建的分片映射种类。 如果你已创建列表映射，请添加列表映射。 如果你已创建范围映射，请添加范围映射。

### <a name="option-1-map-the-data-for-a-list-mapping"></a>选项 1：映射列表映射的数据
通过为每个租户添加列表映射来映射数据。  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>选项 2：映射范围映射的数据
添加所有租户 ID 范围的范围映射 - 数据库关联：

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>步骤 4，选项 3：映射单一数据库上多个租户的数据
对于每个租户，运行 Add-ListMapping（上面的选项 1）。 

## <a name="checking-the-mappings"></a>检查映射
可以使用以下命令来查询现有分片及其关联的映射的信息：  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>摘要
完成设置后，可以开始使用弹性数据库客户端库。 你还可以使用[数据依赖型路由](sql-database-elastic-scale-data-dependent-routing.md)和[多分片查询](sql-database-elastic-scale-multishard-querying.md)。

## <a name="next-steps"></a>后续步骤
从 [Azure SQL DB - 弹性数据库工具脚本](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)获取 PowerShell 脚本。

GitHub 上也提供了这些工具：[Azure/elastic-db-tools](https://github.com/Azure/elastic-db-tools)。

使用拆分/合并工具在多租户模型与单租户模型之间来回移动数据。 请参阅[拆分合并工具](sql-database-elastic-scale-get-started.md)。

## <a name="additional-resources"></a>其他资源
有关多租户软件即服务 (SaaS) 数据库应用程序的常见数据体系结构模式的信息，请参阅 [包含 Azure SQL 数据库的多租户 SaaS 应用程序的设计模式](sql-database-design-patterns-multi-tenancy-saas-applications.md)。

## <a name="questions-and-feature-requests"></a>问题和功能请求
如有问题，请在 [SQL 数据库论坛](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)上联系我们；对于功能请求，请将其添加到 [SQL 数据库反馈论坛](https://feedback.azure.com/forums/217321-sql-database/)。

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

