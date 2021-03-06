---
title: Verwenden von Hadoop Hive mit PowerShell in HDInsight | Microsoft-Dokumentation
description: "Ausführen von Hive-Abfragen mit Hadoop in HDInsight mithilfe von PowerShell"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/07/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 9cf1faabe3ea12af0ee5fd8a825975e30947b03a
ms.openlocfilehash: bb5fee05bb14276ab0a9e2309f8a20cb3684df57


---
# <a name="run-hive-queries-using-powershell"></a>Ausführen von Hive-Abfragen mit PowerShell
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Dieses Dokument enthält ein Beispiel für die Verwendung von Azure PowerShell im Azure-Ressourcengruppenmodus zum Ausführen von Hive-Abfragen in einem Hadoop-Cluster in HDInsight.

> [!NOTE]
> Dieses Dokument bietet keine detaillierte Beschreibung dazu, wie die in diesem Beispiel verwendeten HiveQL-Anweisungen vorgehen. Informationen zu der HiveQL, die in diesem Beispiel verwendet wird, finden Sie unter [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md).
> 
> 

**Voraussetzungen**

Damit Sie die in dieser Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* **Einen Azure HDInsight-Cluster (Hadoop in HDInsight), der auf Windows oder Linux basiert**
* **Eine Arbeitsstation mit Azure PowerShell**.
  
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a>Ausführen von Hive-Abfragen mit Azure PowerShell
Azure PowerShell stellt *cmdlets* bereit, mit denen Sie Hive-Abfragen in HDInsight remote ausführen können. Intern wird dies mithilfe von REST-Aufrufen an [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (früher Templeton) im HDInsight-Cluster realisiert.

Die folgenden Cmdlets werden zum Ausführen der Hive-Abfragen auf einem HDInsight-Remotecluster verwendet:

* **Add-AzureRmAccount**: Authentifiziert Azure PowerShell für Ihr Azure-Abonnement.
* **New-AzureRmHDInsightHiveJobDefinition**: Erstellt mithilfe der angegebenen HiveQL-Anweisungen eine neue *Auftragsdefinition*
* **Start-AzureRmHDInsightJob**: sendet die Auftragsdefinition an HDInsight, startet den Auftrag und gibt ein *Auftrags* -Objekt zurück, mit dem der Status des Auftrags geprüft werden kann
* **Wait-AzureRmHDInsightJob**: verwendet das Auftragsobjekt, um den Status des Auftrags zu prüfen. Es wird gewartet, bis der Auftrag abgeschlossen oder die Wartezeit überschritten ist.
* **Get-AzureRmHDInsightJobOutput**: wird zum Abrufen der Ausgabe des Auftrags verwendet
* **Invoke-AzureRmHDInsightHiveJob**: wird zum Ausführen von HiveQL-Anweisungen verwendet. Dadurch wird die Abfrage vollständig blockiert und anschließend die Ergebnisse zurückgegeben
* **Use-AzureRmHDInsightCluster**: Legt den aktuellen Cluster für die Verwendung mit dem Befehl **Invoke-AzureRmHDInsightHiveJob** fest.

Die folgenden Schritte veranschaulichen, wie diese Cmdlets zum Ausführen eines Auftrags auf dem HDInsight-Cluster verwendet werden:

1. Speichern Sie den folgenden Code mithilfe eines Editors als **hivejob.ps1**. Ersetzen Sie **CLUSTERNAME** durch den Namen des HDInsight-Clusters.
   
        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential
   
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }
   
        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
   
        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 
   
        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green
   
        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds
   
        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds
   
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
2. Öffnen Sie eine neue **Azure PowerShell** -Befehlsaufforderung. Navigieren Sie zum Speicherort der Datei **hivejob.ps1** , und verwenden Sie folgenden Befehl zum Ausführen des Skripts:
   
        .\hivejob.ps1
   
    Beim Ausführen des Skripts werden Sie aufgefordert, die Anmeldeinformationen für das HTTPS-/Administratorkonto für den Cluster einzugeben. Sie werden außerdem möglicherweise dazu aufgefordert, sich bei Ihrem Azure-Abonnement anzumelden.
3. Nachdem der Auftrag abgeschlossen ist, muss er Informationen zurückgeben, die den folgenden ähneln:
   
        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id
4. Wie bereits erwähnt, kann **Invoke-Hive** dazu verwendet werden, um eine Abfrage auszuführen und auf die Antwort zu warten. Verwenden Sie die folgenden Befehle, wobei Sie **CLUSTERNAME** durch den Namen des Clusters ersetzen.
   
        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString
   
    Die Ausgabe sieht dann wie folgt aus:
   
        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id
   
   > [!NOTE]
   > Für längere HiveQL-Abfragen können Sie Azure PowerShell **Here-Strings** -Cmdlet oder HiveQL-Skriptdateien verwenden. Der folgende Ausschnitt zeigt, wie Sie eine HiveQL-Skriptdatei mit dem **Invoke-Hive** -Cmdlet ausführen können. Die HiveQL-Skriptdatei muss auf wasbs:// hochgeladen werden.
   > 
   > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   > 
   > Weitere Informationen zu **Here-Strings** erhalten Sie unter <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Using Windows PowerShell „Here-Strings“</a> (Verwenden von Windows PowerShell-Here-Strings).
   > 
   > 

## <a name="troubleshooting"></a>Problembehandlung
Wenn nach Abschluss des Auftrags keine Informationen zurückgegeben werden, ist während der Verarbeitung möglicherweise ein Fehler aufgetreten. Um Fehlerinformationen für diesen Auftrag anzuzeigen, fügen Sie Folgendes am Ende der Datei **hivejob.ps1** hinzu. Speichern Sie die Datei anschließend, und führen Sie sie erneut aus.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Dadurch werden die beim Ausführen des Auftrags an STDERR auf dem Server geschriebenen Informationen zurückgegeben, die möglicherweise hilfreich sind, um festzustellen, warum der Auftrag fehlgeschlagen ist.

## <a name="summary"></a>Zusammenfassung
Wie Sie sehen können, bietet Azure PowerShell eine einfache Möglichkeit, um Hive-Abfragen auf einem HDInsight-Cluster auszuführen, den Auftragsstatus zu überwachen und die Ausgabe abzurufen.

## <a name="next-steps"></a>Nächste Schritte
Allgemeine Informationen zu Hive in HDInsight:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)




<!--HONumber=Nov16_HO3-->


