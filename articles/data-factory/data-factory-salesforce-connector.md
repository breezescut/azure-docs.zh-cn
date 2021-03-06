---
title: "使用数据工厂从 Salesforce 移动数据 | Microsoft Docs"
description: "了解如何使用 Azure 数据工厂从 Salesforce 移动数据。"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: c5daac3b8374927c094e79299ce52031181ea24d
ms.lasthandoff: 03/15/2017


---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a>使用 Azure 数据工厂从 Salesforce 移动数据
本文概括介绍了如何使用 Azure 数据工厂中的复制活动将数据从本地 Salesforce 移动到[支持的源和接收器](data-factory-data-movement-activities.md#supported-data-stores-and-formats)表中的接收器列下所列出的任何数据存储。 本文基于[数据移动活动](data-factory-data-movement-activities.md)一文，其中总体概述了如何结合使用复制活动和受支持的数据存储进行数据移动。

Azure 数据工厂当前仅支持将数据从 Salesforce 移动到[支持的接收器数据存储](data-factory-data-movement-activities.md#supported-data-stores-and-formats)，不支持将数据从其他数据存储移动到 Salesforce。

## <a name="supported-versions"></a>支持的版本
此连接器支持以下版本的 Salesforce：开发人员版、专业版、企业版或不受限制版。 并支持从 Salesforce 生产、沙盒和自定义域进行复制。

## <a name="prerequisites"></a>先决条件
* 必须启用 API 权限。 请参阅 [How do I enable API access in Salesforce by permission set?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)（如何通过权限集启用 Salesforce 中的 API 访问权限？）
* 若要将数据从 Salesforce 复制到本地数据存储，必须在本地环境中至少安装有数据管理网关 2.0。

## <a name="salesforce-request-limits"></a>Salesforce 请求限制
Salesforce 对 API 请求总数和并发 API 请求均有限制。 注意以下事项：
* 如果并发请求数超过限制，则将进行限制并且你将看到随机失败；
* 如果请求总数超过限制，将阻止 Salesforce 帐户 24 小时。

在这两种情况下，你还可能会收到“REQUEST_LIMIT_EXCEEDED”错误。 有关详细信息，请参阅 [Salesforce Developer Limits](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf)（Salesforce 开发人员限制）文章中的"API 请求限制"一节。

## <a name="copy-data-wizard"></a>复制数据向导
若要创建将数据从 Salesforce 复制到任何支持的接收数据存储的管道，最简单的方法是使用复制数据向导。 请参阅[教程：使用复制向导创建管道](data-factory-copy-data-wizard-tutorial.md)，了解有关使用复制数据向导创建管道的快速演练。

以下示例提供示例 JSON 定义，可使用该定义通过 [Azure 门户](data-factory-copy-activity-tutorial-using-azure-portal.md)、[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) 或 [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) 创建管道。 它们演示了如何将数据从 Salesforce 复制到 Azure Blob 存储。 但是，可使用 Azure 数据工厂中的复制活动将数据复制到[此处](data-factory-data-movement-activities.md#supported-data-stores-and-formats)所述的任何接收器。   

## <a name="sample-copy-data-from-salesforce-to-an-azure-blob"></a>示例：将数据从 Salesforce 复制到 Azure Blob
此示例中每隔一小时会将数据从 Salesforce 复制到 Azure blob。 示例后续部分描述了用于这些示例的 JSON 属性。 可在 Azure 数据工厂中使用复制活动，直接将数据复制到[数据移动活动](data-factory-data-movement-activities.md#supported-data-stores-and-formats)一文中所述的任何接收器中。

以下是要实现方案需要创建的数据工厂项目。 列表后面的部分介绍了有关这些步骤的详细信息。

* [Salesforce](#salesforce-linked-service-properties) 类型的链接服务
* [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) 类型的链接服务
* [RelationalTable](#salesforce-dataset-properties) 类型的输入[数据集](data-factory-create-datasets.md)
* [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) 类型的输出[数据集](data-factory-create-datasets.md)
* 包含复制活动的[管道](data-factory-create-pipelines.md)，该复制活动使用 [RelationalSource](#relationalsource-type-properties) 和 [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)

**Salesforce 链接服务**

此示例使用 **Salesforce** 链接服务。 有关此链接服务支持的属性，请参阅 [Salesforce 链接服务](#salesforce-linked-service-properties)部分。  请参阅[获取安全令牌](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm)了解有关如何重置/获取安全令牌的说明。

```JSON
{
    "name": "SalesforceLinkedService",
    "properties":
    {
        "type": "Salesforce",
        "typeProperties":
        {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```
**Azure 存储链接服务**

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
    }
}
```
**Salesforce 输入数据集**

```JSON
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"  
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

将“external”设置为“true”将告知数据工厂服务：数据集在数据工厂外部且不由数据工厂中的活动生成。

> [!IMPORTANT]
> 任何自定义对象均需要 API 名称的“__c”部分。
>
>

![数据工厂 - Salesforce 连接 - API 名称](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**Azure Blob 输出数据集**

数据将写入到新 blob，每小时进行一次（频率：小时，间隔：1）。

```JSON
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/alltypes_c"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**包含复制活动的管道**

该管道包含配置为使用上述输入和输出数据集的复制活动，且计划每隔一小时运行一次。 在管道 JSON 定义中，将 **source** 类型设置为 **RelationalSource**，将 **sink** 类型设置为 **BlobSink**。

有关 RelationalSource 支持的属性的列表，请参阅 [RelationalSource 类型属性](#relationalsource-type-properties)。

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "SalesforceInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"                
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }
        ]
    }
}
```
> [!IMPORTANT]
> 任何自定义对象均需要 API 名称的“__c”部分。
>

![数据工厂 - Salesforce 连接 - API 名称](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="salesforce-linked-service-properties"></a>Salesforce 链接服务属性
下表提供了针对 Salesforce 链接服务的 JSON 元素说明。

| 属性 | 说明 | 必选 |
| --- | --- | --- |
| type |类型属性必须设置为：**Salesforce** |是 |
| environmentUrl | 指定 Salesforce 实例的 URL。 <br><br> - 默认值为“https://login.salesforce.com”。 <br> - 若要从沙盒复制数据，请指定“https://test.salesforce.com”。 <br> - 若要从自定义域复制数据，请指定“https://[domain].my.salesforce.com”等内容。 |否 |
| username |为用户帐户指定用户名。 |是 |
| password |指定用户帐户的密码。 |是 |
| securityToken |为用户帐户指定安全令牌。 请参阅[获取安全令牌](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm)了解有关如何重置/获取安全令牌的说明。 若要了解有关安全令牌的一般信息，请参阅 [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm)（安全性和 API）。 |是 |

## <a name="salesforce-dataset-properties"></a>Salesforce 数据集属性
有关可用于定义数据集的各节和属性的完整列表，请参阅[创建数据集](data-factory-create-datasets.md)一文。 对于所有数据集类型（Azure SQL、Azure blob、Azure 表等），数据集 JSON 的结构、可用性和策略这些部分类似。

每种数据集的 **TypeProperties** 节有所不同，该部分提供有关数据在数据存储区中的位置信息。 **RelationalTable** 类型的数据集的 typeProperties 部分具有以下属性：

| 属性 | 说明 | 必选 |
| --- | --- | --- |
| tableName |在 Salesforce 中表的名称。 |否（如果指定了 **RelationalSource** 的**query**） |

> [!IMPORTANT]
> 任何自定义对象均需要 API 名称的“__c”部分。
>
>

![数据工厂 - Salesforce 连接 - API 名称](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="relationalsource-type-properties"></a>RelationalSource 类型属性
有关可用于定义活动的各节和属性的完整列表，请参阅[创建管道](data-factory-create-pipelines.md)一文。 名称、说明、输入和输出表等属性和各种策略可用于所有类型的活动。

另一方面，可用于此活动的 typeProperties 节的属性因每个活动类型而异。 对于复制活动，这些属性则因源和接收器的类型而异。

在复制活动中，当源属于 **RelationalSource** 类型（包括 Salesforce）时，以下属性在 typeProperties 部分中可用：

| 属性 | 说明 | 允许的值 | 必选 |
| --- | --- | --- | --- |
| query |使用自定义查询读取数据。 |SQL&92; 查询或 [Salesforce 对象查询语言 (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) 查询。 例如：`select * from MyTable__c`。 |否（如果指定了**数据集**的 **tableName**） |

> [!IMPORTANT]
> 任何自定义对象均需要 API 名称的“__c”部分。
>
>

![数据工厂 - Salesforce 连接 - API 名称](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a>查询提示
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>使用 DateTime 列上的 where 子句检索数据
当指定 SOQL 或 SQL 查询时，请注意 DateTime 的格式差异。 例如：

* **SOQL 示例**：`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`
* **SQL 示例**：
    * **使用复制向导指定查询：**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`
    * **使用 JSON 编辑指定查询（正确转义字符）：**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`

### <a name="retrieving-data-from-salesforce-report"></a>从 Salesforce 报表检索数据
可通过将查询指定为 `{call "<report name>"}` 从 Salesforce 报表检索数据，例如 `"query": "{call \"TestReport\"}"`

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>从 Salesforce 回收站中检索删除的记录
若要从 Salesforce 回收站中检索软删除的记录，需要在查询中指定 **“IsDeleted = 1”**。 例如，

* 若仅查询已删除的记录，请指定“select *from MyTable__c**where IsDeleted= 1**”
* 若要查询包括现有和已删除记录的所有记录，请指定“select *from MyTable__c**where IsDeleted = 0 or IsDeleted = 1**”

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-salesforce"></a>Salesforce 的类型映射
| Salesforce 类型 | 基于 .NET 的类型 |
| --- | --- |
| 自动编号 |String |
| 复选框 |布尔 |
| 货币 |Double |
| 日期 |DateTime |
| 日期/时间 |DateTime |
| 电子邮件 |String |
| ID |String |
| 查找关系 |String |
| 多选择列表 |String |
| Number |Double |
| 百分比 |Double |
| 电话 |String |
| 选择列表 |String |
| 文本 |String |
| 文本区域 |String |
| 文本区域（长型值） |String |
| 文本区域（丰富） |String |
| 文本（加密） |String |
| 代码 |String |

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>性能和优化
若要了解影响 Azure 数据工厂中数据移动（复制活动）性能的关键因素以及各种优化方法，请参阅[复制活动性能和优化指南](data-factory-copy-activity-performance.md)。

