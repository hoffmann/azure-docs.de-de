---
title: Einrichten der Entwicklungsumgebung | Microsoft Docs
description: "Installieren Sie Laufzeit, SDK und Tools, und erstellen Sie einen lokalen Entwicklungscluster. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen beginnen."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/13/2016
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 04092b735fa77c72ffe6c492a3fc975eac2e99fd
ms.openlocfilehash: a71b77a320e9321eaa857acfcfae8822de0ac9e5


---
# <a name="prepare-your-development-environment"></a>Vorbereiten Ihrer Entwicklungsumgebung
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Zum Entwickeln und Ausführen von [Azure Service Fabric-Anwendungen][1] müssen Sie auf dem Entwicklungscomputer die Laufzeit, das SDK und die Tools installieren. Sie müssen auch die Ausführung der im SDK enthaltenen Windows PowerShell-Skripts aktivieren.

## <a name="prerequisites"></a>Voraussetzungen
### <a name="supported-operating-system-versions"></a>Unterstützte Betriebssystemversionen
Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows 10

> [!NOTE]
> Windows 7 enthält standardmäßig nur Windows PowerShell 2.0. Für Service Fabric PowerShell-Cmdlets ist PowerShell 3.0 oder höher erforderlich. Sie können [Windows PowerShell 5.0][powershell5-download] aus dem Microsoft Download Center herunterladen.
> 
> 

## <a name="install-the-runtime-sdk-and-tools"></a>Installieren von Laufzeit, SDK und Tools
Der Webplattform-Installer bietet zwei Konfigurationen für die Service Fabric-Entwicklung.

Visual Studio 2017 (Workload für die Azure-Entwicklung und -Verwaltung muss installiert werden):

* [Installieren der Service Fabric-Laufzeit und des SDKs (ohne Visual Studio-Tools)][core-sdk]

Visual Studio 2015 (erfordert mindestens Visual Studio 2015 Update 2):

* [Installieren der Service Fabric-Laufzeit, des SDKs und der Tools][full-bundle-vs2015]
* [Installieren der Service Fabric-Laufzeit und des SDKs (ohne Visual Studio-Tools)][core-sdk]

> [!WARNING]
> Kunden haben uns darauf hingewiesen, dass bei Verwendung dieser Startlinks Installationsfehler auftreten können (insbesondere bei Verwendung des Chrome-Browsers). Diese Probleme des Webplattform-Installers sind bekannt und werden momentan behoben.  Gehen Sie solange wie folgt vor, um das Problem zu umgehen:
>- Verwenden Sie die obigen Links in Internet Explorer oder Edge. Oder:
>- Starten Sie den Webplattform-Installer über das Startmenü, suchen Sie nach „Service Fabric“, und installieren Sie das SDK.
> 
> Wir entschuldigen uns für die Unannehmlichkeiten. 

Aktuelle Versionen:
* Service Fabric SDK: 2.4.145
* Service Fabric-Laufzeit: 5.4.145
* Visual Studio 2015-Tools: 1.4.41209

Eine Liste mit unterstützten Versionen finden Sie unter [Service Fabric-Unterstützung](service-fabric-support.md).

## <a name="enable-powershell-script-execution"></a>Aktivieren der PowerShell-Skriptausführung
Service Fabric verwendet Windows PowerShell-Skripts zum Erstellen eines lokalen Entwicklungsclusters und zum Bereitstellen von Anwendungen aus Visual Studio. Die Ausführung dieser Skripts wird von Windows standardmäßig blockiert. Um die Skripts zu aktivieren, müssen Sie die PowerShell-Ausführungsrichtlinie ändern. Öffnen Sie PowerShell als Administrator, und geben Sie folgenden Befehl ein:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die Entwicklungsumgebung eingerichtet haben, können Sie nun mit dem Erstellen und Ausführen von Apps beginnen.

* [Erstellen Ihrer ersten Service Fabric-Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
* [Weitere Informationen zum Bereitstellen und Verwalten von Anwendungen in Ihrem lokalen Cluster](service-fabric-get-started-with-a-local-cluster.md)
* [Weitere Informationen zu Programmiermodellen: Reliable Services und Reliable Actors](service-fabric-choose-framework.md)
* [Service Fabric-Codebeispiele auf GitHub](https://aka.ms/servicefabricsamples)
* [Visualisieren des Clusters mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Folgen Sie dem Service Fabric-Lernpfad, um eine umfassende Einführung in die Plattform zu erhalten.](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric-Kampagnenseite"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "WebPI-Link für VS 2015"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "WebPI-Link für Dev15"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "WebPI-Link für Core SDK"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395



<!--HONumber=Dec16_HO2-->


