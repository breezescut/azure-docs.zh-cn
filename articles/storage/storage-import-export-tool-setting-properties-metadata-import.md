---
title: "使用 Azure 导入/导出设置属性和元数据 | Microsoft Docs"
description: "了解如何在运行 Azure 导入/导出工具准备驱动器时，指定要对目标 Blob 设置的属性和元数据。"
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 8de848b1192ff1c10e0375053c4e03f18c06184e
ms.openlocfilehash: 26fb4d030341001238e7206c3856597c95d84719
ms.lasthandoff: 02/16/2017


---

# <a name="setting-properties-and-metadata-during-the-import-process"></a>在导入过程中设置属性和元数据

在运行 Microsoft Azure 导入/导出工具准备驱动器时，可以指定要对目标 Blob 设置的属性和元数据。 执行以下步骤:

1.  若要设置 Blob 属性，请在本地计算机上创建一个指定属性名称和值的文本文件。
2.  若要设置 Blob 元数据，请在本地计算机上创建一个指定元数据名称和值的文本文件。
3.  在 `PrepImport` 操作过程中，需要将其中一个或两个文件的完整路径传递给 Azure 导入/导出工具。

> [!NOTE]
>  将某个属性或元数据文件指定为复制会话的一部分时，将为作为该复制会话一部分导入的每个 Blob 设置这些属性或元数据。 如果想要为导入的某些 Blob 指定一组不同的属性或元数据，需要创建包含不同属性或元数据文件的单独复制会话。

## <a name="specify-blob-properties-in-a-text-file"></a>在文本文件中指定 Blob 属性

若要指定 Blob 属性，请创建一个本地文本文件，同时包含将属性名称指定为元素、将属性值指定为值的 XML。 以下示例演示如何指定一些属性值：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

将该文件保存到本地位置，如 `C:\WAImportExport\ImportProperties.txt`。

## <a name="specify-blob-metadata-in-a-text-file"></a>在文本文件中指定 Blob 元数据

同样，若要指定 Blob 元数据，请创建一个本地文本文件，用于将元数据名称指定为元素、将元数据值指定为值。 以下示例演示如何指定一些元数据值：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export Service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

将该文件保存到本地位置，如 `C:\WAImportExport\ImportMetadata.txt`。

## <a name="add-the-path-to-properties-and-metadata-files-in-datasetcsv"></a>在 dataset.csv 中添加属性和元数据文件的路径

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a>后续步骤

[导入/导出服务元数据和属性文件格式](storage-import-export-file-format-metadata-and-properties.md)

