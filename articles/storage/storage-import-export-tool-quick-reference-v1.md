---
title: "Azure 导入/导出工具的导入作业命令的快速参考 | Microsoft Docs"
description: "导入作业常用的 Azure 导入/导出工具命令参考。 本文所述的导入/导出工具为 v1 版本。"
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
ms.date: 01/15/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 0c58a94a553a22ac06bfdfd8032879f4a4a87fe5
ms.openlocfilehash: e1c440ee165d148b59f29035b853cd8e13a44e7c


---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>导入作业的常用命令快速参考
本部分提供一些常用命令的快速参考。 有关详细用法，请参阅[为导入作业准备硬盘驱动器](storage-import-export-tool-preparing-hard-drives-import-v1.md)。  

## <a name="prepare-the-disks-when-data-already-copied-to-the-disks"></a>在已将数据复制到磁盘的情况下准备磁盘
 以下示例命令演示在已将数据复制到尚未使用 BitLocker 加密的硬盘驱动器的情况下如何准备磁盘：  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a>将单个目录复制到硬盘驱动器  
 以下示例命令演示如何将单个源目录复制到尚未使用 BitLocker 加密的硬盘驱动器：  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-to-a-hard-drive"></a>将 wwo 目录复制到硬盘驱动器  
 若要将两个源目录复制到驱动器，需要运行两个命令。  
  
 第一个命令指定日志目录、存储帐户密钥、目标驱动器号和 `format/encrypt` 要求，以及通用参数：  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 第二个命令指定日志文件、新会话 ID、源位置和目标位置：  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a>在第二个复制会话中将大型文件复制到硬盘驱动器  
 以下示例命令演示如何将单个大型文件复制到已在上一个复制会话中准备的驱动器：  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="see-also"></a>另请参阅  
 [为导入作业准备硬盘驱动器的示例工作流](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)



<!--HONumber=Dec16_HO3-->


