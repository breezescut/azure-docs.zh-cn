---
title: "在 HDInsight 中将 Hadoop Pig 与 PowerShell 配合使用 | Microsoft Docs"
description: "了解如何使用 Azure PowerShell 将 Pig 作业提交到 HDInsight 上的 Hadoop 群集。"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: b4f2f1195887d8b64599e82334ab36b21eef1d1d
ms.lasthandoff: 03/22/2017

---
# <a name="use-azure-powershell-to-run-pig-jobs-with-hdinsight"></a>使用 Azure PowerShell 通过 HDInsight 运行 Pig 作业

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

本文档提供使用 Azure PowerShell 向 HDInsight 群集上的 Hadoop 提交 Pig 作业的示例。 Pig 允许用户通过使用可为数据转换建模的语言 (Pig Latin) 编写 MapReduce 作业，而无需使用映射和化简函数。

> [!NOTE]
> 本文档未详细描述示例中使用的 Pig Latin 语句的作用。 有关此示例中使用的 Pig Latin 的详细信息，请参阅[将 Pig 与 HDInsight 上的 Hadoop 配合使用](hdinsight-use-pig.md)。

## <a id="prereq"></a>先决条件

* **一个 Azure HDInsight 群集**

  > [!IMPORTANT]
  > Linux 是 HDInsight 3.4 或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight Deprecation on Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date)（HDInsight 在 Windows 上即将弃用）。

* **配备 Azure PowerShell 的工作站**。

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a id="powershell"></a>使用 PowerShell 运行 Pig 作业

Azure PowerShell 提供 *cmdlet*，可在 HDInsight 上远程运行 Pig 作业。 PowerShell 在内部使用 REST 调用来调用在 HDInsight 上运行的 [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat)。

在远程 HDInsight 群集上运行 Pig 作业时，将使用以下 Cmdlet：

* **Login-AzureRmAccount**：在 Azure 订阅中进行 Azure PowerShell 身份验证
* **New-AzureRmHDInsightPigJobDefinition**：使用指定的 Pig Latin 语句创建*作业定义*
* **Start-AzureRmHDInsightJob**：将作业定义发送到 HDInsight，启动作业，然后返回可用来检查作业状态的*作业*对象
* **Wait-AzureRmHDInsightJob**：使用作业对象来检查作业的状态。 它将等到作业完成或超出等待时间。
* **Get-AzureRmHDInsightJobOutput**：用于检索作业的输出

以下步骤演示了如何使用这些 Cmdlet 在 HDInsight 群集上运行作业。

1. 使用编辑器将以下代码保存为 **pigjob.ps1**。

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. 打开新的 Windows PowerShell 命令提示符。 将目录更改为 **pigjob.ps1** 文件的所在位置，然后使用以下命令来运行脚本：
   
        .\pigjob.ps1
   
    系统将提示用户登录到 Azure 订阅。 然后，要求输入 HDInsight 群集的 HTTPs/Admin 帐户名和密码。

2. 在作业完成时，它应返回类似于以下文本的信息：
   
        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>故障排除

如果在作业完成时未返回任何信息，则可能表示处理期间发生错误。 若要查看此作业的错误信息，请将以下命令添加到 **pigjob.ps1** 文件的末尾，保存，然后重新运行该文件。

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

在运行作业时，这将返回写入到服务器上的 STDERR 的信息，它可帮助确定该作业失败的原因。

## <a id="summary"></a>摘要
如你所见，Azure PowerShell 提供了简单的方法让你在 HDInsight 群集上运行 Pig 作业、监视作业状态，以及检索输出。

## <a id="nextsteps"></a>后续步骤
有关 HDInsight 中的 Pig 的一般信息：

* [将 Pig 与 Hadoop on HDInsight 配合使用](hdinsight-use-pig.md)

有关 HDInsight 上的 Hadoop 的其他使用方法的信息：

* [将 Hive 与 Hadoop on HDInsight 配合使用](hdinsight-use-hive.md)
* [将 MapReduce 与 HDInsight 上的 Hadoop 配合使用](hdinsight-use-mapreduce.md)


