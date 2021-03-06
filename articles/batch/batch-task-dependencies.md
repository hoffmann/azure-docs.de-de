---
title: "Taskabhängigkeiten in Azure Batch | Microsoft Docs"
description: "Erstellen Sie Tasks, die vom erfolgreichen Abschluss anderer Tasks abhängig sind, um Vorgänge vom MapReduce-Typ und ähnliche Big Data-Workloads in Azure Batch zu verarbeiten."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: b8d12db5-ca30-4c7d-993a-a05af9257210
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 01/05/2017
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: dfcf1e1d54a0c04cacffb50eca4afd39c6f6a1b1
ms.openlocfilehash: 5883417c6f7a0ce45c9c34ac2d37e5c1bea95ab1


---
# <a name="task-dependencies-in-azure-batch"></a>Taskabhängigkeiten in Azure Batch
Das Feature für Taskabhängigkeiten in Azure Batch eignet sich hervorragend für die Verarbeitung der folgenden Workloads und Aufträge:

* Workloads vom Typ MapReduce in der Cloud.
* Aufträge, deren Datenverarbeitungstasks als gerichteter azyklischer Graph (DAG) ausgedrückt werden können.
* Alle anderen Aufträge, in denen nachgelagerte Tasks von der Ausgabe vorgelagerter Tasks abhängen.

Mit Taskabhängigkeiten in Batch können Sie Tasks erstellen, deren Ausführung auf Computeknoten erst nach dem erfolgreichen Abschluss von mindestens einem anderen Task geplant wird. Beispielsweise können Sie einen Auftrag erstellen, der jedes Einzelbild eines 3D-Films mit separaten, parallelen Tasks rendert. Der letzte Task – der „Zusammenführungstask“ – fügt die gerenderten Einzelbilder erst dann zu einem vollständigen Film zusammen, wenn alle Einzelbilder erfolgreich gerendert wurden.

Sie können Tasks erstellen, die in einer&1;:1- oder&1;:n-Beziehung von anderen Tasks abhängen. Sie können sogar eine Bereichsabhängigkeit erstellen, bei der ein Task vom erfolgreichen Abschluss einer Gruppe von Tasks innerhalb eines bestimmten Task-ID-Bereichs abhängig ist. Sie können diese drei grundlegenden Szenarien auch kombinieren, um m:n-Beziehungen zu erstellen.

## <a name="task-dependencies-with-batch-net"></a>Taskabhängigkeiten bei Batch .NET
In diesem Artikel wird beschrieben, wie Sie Taskabhängigkeiten mit der [.NET-Bibliothek für Batch][net_msdn] konfigurieren. Zuerst zeigen wir Ihnen, wie Sie die [Taskabhängigkeit in Ihren Aufträgen aktivieren](#enable-task-dependencies), danach erläutern wir, wie Sie [einen Task mit Abhängigkeiten konfigurieren](#create-dependent-tasks). Zuletzt geht es um die von Batch unterstützten [Abhängigkeitsszenarien](#dependency-scenarios) .

## <a name="enable-task-dependencies"></a>Aktivieren von Taskabhängigkeiten
Um Taskabhängigkeiten in der Batch-Anwendung verwenden zu können, müssen Sie dem Batch-Dienst zuerst mitteilen, dass für den Auftrag Taskabhängigkeiten verwendet werden. Führen Sie die Aktivierung in Batch .NET für Ihr [CloudJob][net_cloudjob]-Objekt durch, indem Sie die dazugehörige [UsesTaskDependencies][net_usestaskdependencies]-Eigenschaft auf `true` festlegen:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Im obigen Codeausschnitt ist „batchClient“ eine Instanz der [BatchClient][net_batchclient]-Klasse.

## <a name="create-dependent-tasks"></a>Erstellen von abhängigen Tasks
Zum Erstellen eines Tasks, der vom erfolgreichen Abschluss von mindestens eines anderen Tasks abhängig ist, legen Sie in Batch fest, dass der Task von den anderen Tasks abhängt („Abhängig von“). Konfigurieren Sie in Batch .NET die [CloudTask][net_cloudtask].[DependsOn][net_dependson]-Eigenschaft mit einer Instanz der [TaskDependencies][net_taskdependencies]-Klasse:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Mit diesem Codeausschnitt wird ein Task mit der ID „Flowers“ erstellt, dessen Ausführung auf einem Computeknoten erst dann geplant ist, nachdem die Tasks mit den IDs „Rain“ und „Sun“ erfolgreich abgeschlossen wurden.

> [!NOTE]
> Ein Task wird als abgeschlossen angesehen, wenn er den Zustand **abgeschlossen** aufweist und der **Exitcode** den Wert `0` hat. In Batch .NET bedeutet dies, dass der [CloudTask][net_cloudtask].[State][net_taskstate]-Eigenschaftswert `Completed` und der [TaskExecutionInformation][net_taskexecutioninformation].[ExitCode][net_exitcode]-Eigenschaftswert von „CloudTask“ `0` lautet.
> 
> 

## <a name="dependency-scenarios"></a>Abhängigkeitsszenarien
Es gibt drei grundlegende Szenarien für Abhängigkeiten von Tasks, die Sie in Azure Batch verwenden können:&1;:1,&1;:n und Task-ID-Bereich. Diese können Sie kombinieren, um ein viertes Szenario zu schaffen, und zwar „m:n“.

| Szenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Beispiel |  |
|:---:| --- | --- |
|  [1:1](#one-to-one) |*TaskB* hängt von *TaskA* ab. <p/> *TaskB* wird erst ausgeführt, nachdem *TaskA* erfolgreich abgeschlossen wurde |![Diagramm:&1;:1-Abhängigkeit von Tasks][1] |
|  [1:n](#one-to-many) |*TaskC* hängt sowohl von *TaskA* als auch von *TaskB* ab. <p/> *TaskC* wird erst ausgeführt, nachdem *TaskA* und *TaskB* erfolgreich abgeschlossen wurden. |![Diagramm:&1;:n-Abhängigkeit von Tasks][2] |
|  [Task-ID-Bereich](#task-id-range) |*TaskD* hängt von einem Taskbereich ab. <p/> *TaskD* wird erst ausgeführt, nachdem die Tasks mit den IDs *1* bis *10* erfolgreich abgeschlossen wurden. |![Diagramm: Abhängigkeit vom Task-ID-Bereich][3] |

> [!TIP]
> Sie können **m:n**-Beziehungen erstellen, bei denen beispielsweise die Tasks C, D, E und F jeweils von den Tasks A und B abhängen. Dies ist beispielsweise für Fälle mit parallelisierter Vorverarbeitung nützlich, in denen Ihre nachgelagerten Tasks von der Ausgabe mehrerer vorgelagerter Tasks abhängig sind.
> 
> 

### <a name="one-to-one"></a>1:1
Um einen Task mit einer Abhängigkeit von der erfolgreichen Ausführung eines anderen Tasks zu erstellen, stellen Sie eine einzelne Task-ID für die statische [TaskDependencies][net_taskdependencies].[OnId][net_onid]-Methode bereit, wenn Sie die [DependsOn][net_dependson]-Eigenschaft von [CloudTask][net_cloudtask] auffüllen.

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>1:n
Um einen Task mit einer Abhängigkeit von der erfolgreichen Ausführung mehrerer Tasks zu erstellen, stellen Sie eine Sammlung von Task-IDs für die statische [TaskDependencies][net_taskdependencies].[OnIds][net_onids]-Methode bereit, wenn Sie die [DependsOn][net_dependson]-Eigenschaft von [CloudTask][net_cloudtask] auffüllen.

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Task-ID-Bereich
Um einen Task mit einer Abhängigkeit von der erfolgreichen Ausführung einer Gruppe von Tasks zu erstellen, deren IDs innerhalb eines bestimmten Bereichs liegen, stellen Sie die erste und die letzte Task-ID des Bereichs für die statische [TaskDependencies][net_taskdependencies].[OnIdRange][net_onidrange]-Methode bereit, wenn Sie die [DependsOn][net_dependson]-Eigenschaft von [CloudTask][net_cloudtask] auffüllen.

> [!IMPORTANT]
> Wenn Sie Task-ID-Bereiche für Ihre Abhängigkeiten verwenden, *muss* es sich bei den Task-IDs im Bereich um Zeichenfolgendarstellungen von Ganzzahlwerten handeln. Außerdem muss jeder Task im Bereich erfolgreich abgeschlossen werden, damit die Ausführung des abhängigen Tasks geplant wird.
> 
> 

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Codebeispiel
Das Beispielprojekt [TaskDependencies][github_taskdependencies] ist eines der [Azure Batch-Codebeispiele][github_samples] auf GitHub. Mit dieser Visual Studio 2015-Projektmappe wird veranschaulicht, wie Sie die Taskabhängigkeit für einen Auftrag aktivieren, Tasks erstellen, die von anderen Tasks abhängen, und diese Tasks in einem Pool aus Computeknoten ausführen.

## <a name="next-steps"></a>Nächste Schritte
### <a name="application-deployment"></a>Anwendungsbereitstellung
Das Batch-Feature [Anwendungspakete](batch-application-packages.md) bietet eine einfache Möglichkeit zum Bereitstellen und Versionieren der Anwendungen, die von Ihren Tasks auf Computeknoten ausgeführt werden.

### <a name="installing-applications-and-staging-data"></a>Installieren von Anwendungen und Bereitstellen von Daten (Staging)
Der Beitrag [Installing applications and staging data on Batch compute nodes][forum_post] (Installieren von Anwendungen und Bereitstellen von Daten auf Batch-Computeknoten) im Azure Batch-Forum bietet einen Überblick über die verschiedenen Methoden, mit denen Knoten für die Ausführung von Tasks vorbereitet werden können. Der Beitrag wurde von einem Mitglied des Azure Batch-Teams verfasst und enthält alle relevanten Informationen, um sich mit den verschiedenen Vorgehensweisen vertraut zu machen, mit denen sich Dateien (einschließlich Anwendungen und Taskeingabedaten) auf Computeknoten bereitstellen lassen.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagramm:&1;:1-Abhängigkeit"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagramm:&1;:n-Abhängigkeit"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagramm: Abhängigkeit vom Task-ID-Bereich"



<!--HONumber=Dec16_HO2-->


