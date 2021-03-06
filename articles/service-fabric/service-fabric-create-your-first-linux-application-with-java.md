---
title: Erstellen Ihrer ersten Service Fabric-Anwendung unter Linux mithilfe von Java | Microsoft Docs
description: Erstellen und Bereitstellen einer Service Fabric-Anwendung mithilfe von Java
services: service-fabric
documentationcenter: java
author: seanmck
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 381d372b549fa0ed0900d97c03b9584b21624b25
ms.openlocfilehash: 4a8fb3499ec55e451b54a05d5642bdf9a924294f


---
# <a name="create-your-first-azure-service-fabric-application"></a>Erstellen Ihrer ersten Azure Service Fabric-Anwendung
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Service Fabric bietet SDKs, mit denen sich Dienste unter Linux sowohl in .NET Core als auch in Java erstellen lassen. In diesem Tutorial erfahren Sie, wie Sie unter Verwendung von Java eine Anwendung für Linux und einen Dienst erstellen.  

> [!NOTE]
> Java als erstklassige integrierte Programmiersprache wird nur für die Linux-Vorschauversion unterstützt (Windows-Unterstützung ist geplant). Allerdings können alle Anwendungen, einschließlich Java-Anwendungen, als ausführbare Gastdateien oder in Containern unter Windows oder Linux ausgeführt werden. Weitere Informationen finden Sie unter [Bereitstellen einer ausführbaren Gastanwendungsdatei in Service Fabric](service-fabric-deploy-existing-app.md) und [Vorschau: Bereitstellen eines Containers in Service Fabric](service-fabric-deploy-container.md).
>

## <a name="video-tutorial"></a>Videotutorial

Im folgenden Microsoft Virtual Academy-Video wird die Vorgehensweise zum Erstellen einer Java-App unter Linux veranschaulicht:  
<center><a target="\_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-create-your-first-linux-application-with-java/LinuxVid.png" WIDTH="360" HEIGHT="244">  
</a></center>


## <a name="prerequisites"></a>Voraussetzungen
Vor Beginn des Tutorials müssen Sie zunächst [Ihre Linux-Entwicklungsumgebung einrichten](service-fabric-get-started-linux.md). Bei Verwendung von Mac OS X können Sie [mithilfe von Vagrant eine Linux-One-Box-Umgebung auf einem virtuellen Computer einrichten](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Erstellen der Anwendung
Eine Service Fabric-Anwendung kann einen oder mehrere Dienste enthalten, die jeweils eine bestimmte Rolle bei der Bereitstellung von Funktionen der Anwendung haben. Das Service Fabric SDK für Linux enthält ein [Yeoman](http://yeoman.io/) -Generator, mit dem Sie problemlos Ihren ersten Dienst erstellen und später weitere Dienste hinzufügen können. Im nächsten Schritt erstellen wir mithilfe von Yeoman eine Anwendung mit einem einzelnen Dienst.

1. Geben Sie in einem Terminal ``yo azuresfjava`` ein.
2. Benennen Sie Ihre Anwendung.
3. Wählen Sie die Art Ihres ersten Diensts aus, und benennen Sie ihn. Im Rahmen dieses Tutorials wählen wir einen Reliable Actor-Dienst aus.

   ![Service Fabric-Yeoman-Generator für Java][sf-yeoman]

> [!NOTE]
> Weitere Informationen zu den Optionen finden Sie unter [Übersicht über die Service Fabric-Programmiermodelle](service-fabric-choose-framework.md).
>

## <a name="build-the-application"></a>Erstellen der Anwendung
Die Yeoman-Vorlagen von Service Fabric enthalten ein Buildskript für [Gradle](https://gradle.org/). Damit können Sie die App über das Terminal erstellen.

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung
Die erstellte Anwendung kann mithilfe der Azure-Befehlszeilenschnittstelle im lokalen Cluster bereitgestellt werden.

1. Stellen Sie eine Verbindung mit dem lokalen Service Fabric-Cluster her.

    ```bash
    azure servicefabric cluster connect
    ```

2. Verwenden Sie das in der Vorlage bereitgestellte Installationsskript, um das Anwendungspaket in den Imagespeicher des Clusters zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

    ```bash
    ./install.sh
    ```

3. Navigieren Sie in einem Browser zu Service Fabric Explorer (http://localhost:19080/Explorer). Falls Sie Vagrant unter Mac OS X verwenden, ersetzen Sie „localhost“ durch die private IP-Adresse des virtuellen Computers.

4. Erweitern Sie den Knoten „Anwendungen“. Hier finden Sie nun einen Eintrag für Ihren Anwendungstyp und einen weiteren für die erste Instanz dieses Typs.

## <a name="start-the-test-client-and-perform-a-failover"></a>Starten des Testclients und Ausführen eines Failovers
Actor-Projekte führen keine eigenständigen Aktionen durch. Sie benötigen einen anderen Dienst oder Client, der ihnen Nachrichten sendet. Die Actor-Vorlage enthält ein einfaches Testskript, das Sie für die Interaktion mit dem Actor-Dienst verwenden können.

1. Führen Sie das Skript mithilfe des watch-Hilfsprogramms aus, um die Ausgabe des Actor-Diensts zu erhalten.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Suchen Sie in Service Fabric Explorer den Knoten, der das primäre Replikat für den Actor-Dienst hostet. Im folgenden Screenshot ist das „Node_3“:

    ![Suchen des primären Replikats in Service Fabric Explorer][sfx-primary]

3. Klicken Sie auf den Knoten, den Sie im vorherigen Schritt ermittelt haben, und wählen Sie im Aktionsmenü die Option **Deaktivieren (neu starten)** aus. Mit dieser Aktion wird einer der fünf Knoten in Ihrem lokalen Cluster neu gestartet und ein Failover auf eines der sekundären Replikate erzwungen, das auf einem anderen Knoten ausgeführt wird. Behalten Sie bei dieser Aktion die Ausgabe des Testclients im Auge, und beachten Sie, dass sich der Zähler trotz des Failovers weiter erhöht.

## <a name="build-and-deploy-an-application-with-the-eclipse-neon-plugin"></a>Erstellen und Bereitstellen einer Anwendung mit dem Eclipse Neon-Plug-In

Wenn Sie das [Service Fabric-Plug-In](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#install-the-java-sdk-and-eclipse-neon-plugin-optional) für Eclipse Neon installiert haben, können Sie damit Java-basierte Service Fabric-Anwendungen erstellen und bereitstellen.  Wählen Sie bei der Installation von Eclipse die **Eclipse-IDE für Java-Entwickler**.

### <a name="create-the-application"></a>Erstellen der Anwendung

Das Service Fabric-Plug-In ist im Rahmen der Erweiterbarkeit von Eclipse verfügbar.

1. Wählen Sie in Eclipse **Datei > Sonstiges > Service Fabric** aus. Daraufhin wird eine Reihe von Optionen angezeigt. Hierzu zählen auch Actors und Container.

    ![Service Fabric-Vorlagen in Eclipse][sf-eclipse-templates]

2. Wählen Sie in diesem Szenario die Option für den zustandslosen Dienst aus.

3. Sie werden aufgefordert, die Verwendung der Service Fabric-Perspektive zu bestätigen. Dadurch wird Eclipse für die Verwendung mit Service Fabric-Projekten optimiert. Wählen Sie die Option „Ja“ aus.

### <a name="deploy-the-application"></a>Bereitstellen der Anwendung
Die Service Fabric-Vorlagen enthalten eine Reihe von Gradle-Aufgaben zum Erstellen und Bereitstellen von Anwendungen, die Sie über Eclipse auslösen können.

1. Wählen Sie **Ausführen > Run Configurations** (Konfigurationen ausführen) aus.
2. Geben Sie **local** (lokal) oder **cloud** (Cloud) an. Standardmäßig ist **local** (lokal) angegeben. Wählen Sie zur Bereitstellung für einen Remotecluster die Option **cloud** (Cloud) aus.
3. Stellen Sie sicher, dass die Veröffentlichungsprofile die korrekten Angaben enthalten, indem Sie nach Bedarf entweder `local.json` oder `cloud.json` bearbeiten.
4. Klicken Sie auf **Run**(Ausführen).

Ihre App wird innerhalb weniger Augenblicke erstellt und bereitgestellt. Der Status kann über Service Fabric Explorer verfolgt werden.

## <a name="adding-more-services-to-an-existing-application"></a>Hinzufügen weiterer Dienste zu einer vorhandenen Anwendung

Führen Sie zum Hinzufügen eines weiteren Diensts zu einer Anwendung, die bereits mit `yo` erstellt wurde, die folgenden Schritte aus:
1. Legen Sie das Verzeichnis auf den Stamm der vorhandenen Anwendung fest.  Beispiel: `cd ~/YeomanSamples/MyApplication`, wenn `MyApplication` die von Yeoman erstellte Anwendung ist.
2. Führen Sie `yo azuresfjava:AddService` aus.


## <a name="next-steps"></a>Nächste Schritte
* [Erfahren Sie mehr über Reliable Actors.](service-fabric-reliable-actors-introduction.md)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)
* [Problembehandlung bei der Bereitstellung](service-fabric-azure-cli.md#troubleshooting)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png



<!--HONumber=Jan17_HO1-->


