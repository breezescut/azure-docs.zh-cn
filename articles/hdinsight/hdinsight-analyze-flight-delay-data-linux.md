---
title: "在基于 Linux 的 HDInsight 上使用 Hive 分析航班延误数据 | Microsoft Docs"
description: "了解如何在基于 Linux 的 HDInsight 上使用 Hive 分析航班数据，然后使用 Sqoop 将数据导出到 SQL 数据库中。"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0c23a079-981a-4079-b3f7-ad147b4609e5
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: bd3032b3df92c43b6cc6431eff19bd7cc0cc47bd
ms.lasthandoff: 03/25/2017

---
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a>在基于 Linux 的 HDInsight 上使用 Hive 分析航班延误数据

了解如何在基于 Linux 的 HDInsight 上使用 Hive 分析航班延误数据，然后使用 Sqoop 将数据导出到 Azure SQL 数据库中。

> [!IMPORTANT]
> 本文档中的步骤需要使用 Linux 的 HDInsight 群集。 Linux 是 HDInsight 3.4 或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight Deprecation on Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date)（HDInsight 在 Windows 上即将弃用）。

### <a name="prerequisites"></a>先决条件

* **一个 HDInsight 群集**。 有关创建新的基于 Linux 的 HDInsight 群集的步骤，请参阅[在 Linux 上的 HDInsight 中开始将 Hadoop 与 Hive 配合使用](hdinsight-hadoop-linux-tutorial-get-started.md)。

* **Azure SQL 数据库**。 使用 Azure SQL 数据库作为目标数据存储。 如果没有 SQL 数据库，请参阅 [SQL 数据库教程：几分钟内即可创建 SQL 数据库](../sql-database/sql-database-get-started.md)。

* **Azure CLI**。 如果尚未安装 Azure CLI，请参阅[安装和配置 Azure CLI](../cli-install-nodejs.md) 可了解详细步骤。

## <a name="download-the-flight-data"></a>下载航班数据

1. 浏览到[美国研究与技术创新管理部门、运输统计局][rita-website]。

2. 在该页面上，选择以下值：
   
   | Name | 值 |
   | --- | --- |
   | 筛选年份 |2013 |
   | 筛选期间 |1 月 |
   | 字段 |Year、FlightDate、UniqueCarrier、Carrier、FlightNum、OriginAirportID、Origin、OriginCityName、OriginState、DestAirportID、Dest、DestCityName、DestState、DepDelayMinutes、ArrDelay、ArrDelayMinutes、CarrierDelay、WeatherDelay、NASDelay、SecurityDelay、LateAircraftDelay。 清除所有其他字段 |

3. 单击“下载”。 

## <a name="upload-the-data"></a>上载数据

1. 使用以下命令将该 zip 文件上载到 HDInsight 群集头节点：
   
    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```
   
    将 **FILENAME** 替换为 zip 文件的名称。 将 **USERNAME** 替换为 HDInsight 群集的 SSH 登录名。 将 CLUSTERNAME 替换为 HDInsight 群集的名称。
   
   > [!NOTE]
   > 如果使用密码对 SSH 登录名进行身份验证，则系统会提示输入密码。 如果你使用了公钥，则可能需要使用 `-i` 参数并指定匹配私钥的路径。 例如，`scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`。

2. 上载完成后，使用 SSH 连接到群集：
   
    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```
   
    有关详细信息，请参阅 [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)（对 HDInsight 使用 SSH）。

3. 连接后，使用以下命令来解压缩 .zip 文件：
   
    ```
    unzip FILENAME.zip
    ```
   
    此命令会提取大小约为 60 MB 的 .csv 文件。

4. 使用以下命令在 HDInsight 存储上创建目录，然后将文件复制到该目录：
   
    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-the-hiveql"></a>创建并运行 HiveQL

使用以下步骤将 CSV 文件中的数据导入到名为 **Delays** 的 Hive 表中。

1. 使用以下命令创建名为 **flightdelays.hql** 的新文件并编辑它：
   
    ```
    nano flightdelays.hql
    ```
   
    将以下文本用作此文件的内容：
   
    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over the csv file
    CREATE EXTERNAL TABLE delays_raw (
        YEAR string,
        FL_DATE string,
        UNIQUE_CARRIER string,
        CARRIER string,
        FL_NUM string,
        ORIGIN_AIRPORT_ID string,
        ORIGIN string,
        ORIGIN_CITY_NAME string,
        ORIGIN_CITY_NAME_TEMP string,
        ORIGIN_STATE_ABR string,
        DEST_AIRPORT_ID string,
        DEST string,
        DEST_CITY_NAME string,
        DEST_CITY_NAME_TEMP string,
        DEST_STATE_ABR string,
        DEP_DELAY_NEW float,
        ARR_DELAY_NEW float,
        CARRIER_DELAY float,
        WEATHER_DELAY float,
        NAS_DELAY float,
        SECURITY_DELAY float,
        LATE_AIRCRAFT_DELAY float)
    -- The following lines describe the format and location of the file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION '/tutorials/flightdelays/data';

    -- Drop the delays table if it exists
    DROP TABLE delays;
    -- Create the delays table and populate it with data
    -- pulled in from the CSV file (via the external table defined previously)
    CREATE TABLE delays AS
    SELECT YEAR AS year,
        FL_DATE AS flight_date,
        substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
        substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
        substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
        ORIGIN_AIRPORT_ID AS origin_airport_id,
        substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
        substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
        substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
        DEST_AIRPORT_ID AS dest_airport_id,
        substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
        substring(DEST_CITY_NAME,2) AS dest_city_name,
        substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
        DEP_DELAY_NEW AS dep_delay_new,
        ARR_DELAY_NEW AS arr_delay_new,
        CARRIER_DELAY AS carrier_delay,
        WEATHER_DELAY AS weather_delay,
        NAS_DELAY AS nas_delay,
        SECURITY_DELAY AS security_delay,
        LATE_AIRCRAFT_DELAY AS late_aircraft_delay
    FROM delays_raw;
    ```

2. 使用 **Ctrl+X**，然后使用 **Y** 以保存该文件。

3. 使用以下命令启动 Hive 并运行 **flightdelays.hql** 文件：
   
    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
    ```
   
   > [!NOTE]
   > 在此示例中，由于已连接到运行 HiveServer2 的 HDInsight 群集的头节点，因此使用 `localhost`。

4. __flightdelays.hql__ 脚本完成运行后，使用以下命令打开交互式 Beeline 会话：
   
    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

5. 收到 `jdbc:hive2://localhost:10001/>` 提示时，请使用以下查询从导入的航班延误数据中检索数据。
   
    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```
   
    此查询会检索遇到天气延迟的城市的列表，以及平均延迟时间并将其保存到 `/tutorials/flightdelays/output` 中。 稍后，Sqoop 会从该位置读取数据并将其导出到 Azure SQL 数据库中。

6. 若要退出 Beeline，请在提示符处输入 `!quit`。

## <a name="create-a-sql-database"></a>创建 SQL 数据库

如果已具备 SQL 数据库，则必须获取服务器名称。 可通过选择“SQL 数据库”在 [Azure 门户](https://portal.azure.com)中找到服务器名称，然后筛选要使用的数据库的名称。 服务器名称在“SERVER”列中列出。

如果还没有 SQL 数据库，请使用 [SQL 数据库教程：几分钟内即可创建 SQL 数据库](../sql-database/sql-database-get-started.md)中的信息创建一个。 需要保存数据库所使用的服务器名称。

## <a name="create-a-sql-database-table"></a>创建 SQL 数据库表

> [!NOTE]
> 有多种方法可连接到 SQL 数据库并创建表。 以下步骤从 HDInsight 群集中使用 [FreeTDS](http://www.freetds.org/)。


1. 使用 SSH 连接到基于 Linux 的 HDInsight 群集，并从 SSH 会话运行以下步骤。

2. 使用以下命令安装 FreeTDS：
   
    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. 安装完成后，使用以下命令连接到 SQL 数据库服务器。 使用 SQL 数据库服务器名称替换 **serverName**。 使用 SQL 数据库登录信息替换 **adminLogin** 和 **adminPassword**。 使用数据库名称替换 **databaseName**。
   
    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```
   
    将收到类似于以下文本的输出：
   
    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to sqooptest
    1>
    ```

4. 在 `1>` 提示符下，输入以下行：
   
    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```
   
    输入 `GO` 语句后，将评估前面的语句。 这会创建一个名为 **delays** 且具有聚集索引的表。
   
    使用以下命令验证是否已创建该表：
   
    ```
    SELECT * FROM information_schema.tables
    GO
    ```
   
    输出与下面类似：
   
    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. 在 `1>` 提示符下输入 `exit` 以退出 tsql 实用工具。

## <a name="export-data-with-sqoop"></a>使用 Sqoop 导出数据

1. 使用以下命令验证 Sqoop 是否可以看到你的 SQL 数据库：
   
    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```
   
    此命令会返回数据库列表，其中包括此前创建的 delays 表所在的数据库。

2. 使用以下命令将 hivesampletable 中的数据导出到 mobiledata 表：
   
    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```
   
    Sqoop 连接到包含 delays 表的数据库，并将数据从 `/tutorials/flightdelays/output` 目录导出到 delays 表。

3. 该命令完成后，使用以下命令通过 TSQL 连接到数据库：
   
    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```
   
    连接成功以后，使用以下语句验证数据是否已导出到 mobiledata 表：
   
    ```
    SELECT * FROM delays
    GO
    ```
    
    你会在表中看到一系列数据。 键入 `exit` 退出 tsql 实用程序。

## <a id="nextsteps"></a>后续步骤

若要详细了解如何通过各种方式使用 HDInsight 中的数据，请参阅以下文档：

* [将 Hive 与 HDInsight 配合使用][hdinsight-use-hive]
* [将 Oozie 与 HDInsight 配合使用][hdinsight-use-oozie]
* [将 Sqoop 与 HDInsight 配合使用][hdinsight-use-sqoop]
* [将 Pig 与 HDInsight 配合使用][hdinsight-use-pig]
* [为 HDInsight 开发 Java MapReduce 程序][hdinsight-develop-mapreduce]
* [为 HDInsight 开发 Python Hadoop 流式处理程序][hdinsight-develop-streaming]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx




