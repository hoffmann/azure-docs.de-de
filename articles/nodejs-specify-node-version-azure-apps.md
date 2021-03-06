---
title: Festlegen einer Node.js-Version
description: "Enthält Informationen zum Angeben der von Azure Websites und Cloud Services verwendete Node.js-Version"
services: 
documentationcenter: nodejs
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d0e15278-2ab4-4ec8-8256-913839c6d5ef
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/01/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 9b2d456d8dba33af224ea147f5f8ec49ba7397f9
ms.openlocfilehash: 8990d1d903cb0c168b39f3ed164043100f68851c


---
# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Festlegen einer Node.js-Version in einer Azure-Anwendung
Beim Hosten von Node.js-Anwendungen sollten Sie sicherstellen, dass Ihre Anwendung eine bestimmte Version von Node.js verwendet. Es gibt mehrere Möglichkeiten, dies für Anwendungen unter Azure zu erreichen.

## <a name="default-versions"></a>Standardversionen
Die von Azure bereitgestellten Node.js-Versionen werden ständig aktualisiert. Sofern nicht anders angegeben wird die in der Umgebungsvariablen `WEBSITE_NODE_DEFAULT_VERSION` bereitgestellte Standardversion verwendet. Führen Sie zum Überschreiben dieses Standardwerts die Schritte in den folgenden Abschnitten des Artikels aus.

> [!NOTE]
> Wenn Sie Ihre Anwendung in einem Azure Cloud Service hosten (Web- oder Workerrolle) und Sie Ihre Anwendung zum ersten Mal bereitstellen, versucht Azure, dieselbe Node.js-Version wie in Ihrer Entwicklungsumgebung zu verwenden, falls diese mit einer der in Azure verfügbaren Standardversionen übereinstimmt.
>
>

## <a name="versioning-with-packagejson"></a>Versionsverwaltung mit package.json
Sie können den folgenden Code zu Ihrer **package.json** -Datei hinzufügen, um die zu verwendende Node.js-Version anzugeben:

    "engines":{"node":version}

Wobei *version* die zu verwendende Versionsnummer ist. Sie können auch komplexere Bedingungen für die Version angeben, z.B.:

    "engines":{"node": "0.6.22 || 0.8.x"}

Da 0.6.22 keine der Standardversionen in der Hostumgebung ist, wird stattdessen die höchste verfügbare Version der 0.8-Reihe verwendet: 0.8.4.

## <a name="versioning-websites-with-app-settings"></a>Versionsverwaltung von Websites mit App-Einstellungen
Wenn Sie die Anwendung auf einer Website hosten, können Sie die Umgebungsvariable **WEBSITE_NODE_DEFAULT_VERSION** auf die gewünschte Version festlegen.

## <a name="versioning-cloud-services-with-powershell"></a>Versionsverwaltung von Clouddiensten mit PowerShell
Wenn Sie Ihre Anwendung in einem Clouddienst hosten und mit Azure PowerShell bereitstellen, können Sie die verwendete Node.js-Version mithilfe des **Set-AzureServiceProjectRole** PowerShell-Cmdlets überschreiben. Beispiel:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Beachten Sie, dass bei den Parametern in der obigen Anweisung die Groß- und Kleinschreibung beachtet werden muss.  Sie können überprüfen, ob die richtige Version von „Node.js“ ausgewählt wurde, indem Sie die **engines**-Eigenschaft in **package.json** für Ihre Rolle prüfen.

Außerdem können Sie **Get-AzureServiceProjectRoleRuntime** verwenden, um eine Liste der verfügbaren Node.js-Versionen für Anwendungen abzurufen, die als Clouddienste gehostet werden.  Überprüfen Sie anhand dieser Liste die Version von Node.js, von der Ihr Projekt abhängt.

## <a name="using-a-custom-version-with-azure-websites"></a>Verwenden benutzerdefinierter Versionen mit Azure-Websites
Obwohl Azure verschiedene Standardversionen von Node.js anbietet, kann es sein, dass Sie eine andere Version verwenden möchten. Falls Ihre Anwendung als Azure-Website gehostet wird, können Sie dies in der Datei **iisnode.yml** angeben. Die folgenden Schritte beschreiben, wie Sie eine benutzerdefinierte Version von Node.js mit einer Azure-Website verwenden:

1. Erstellen Sie ein neues Verzeichnis und erstellen Sie eine Datei **server.js** in diesem Verzeichnis. Die Datei **server.js** sollte den folgenden Code enthalten:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Dieser Code zeigt beim Aufrufen der Website die verwendete Node.js-Version an.
2. Erstellen Sie eine neue Website, und notieren Sie sich deren Namen. Im folgenden Beispiel werden die [Azure-Befehlszeilentools] verwendet, um eine neue Azure-Website mit dem Namen **mywebsite**zu erstellen und anschließend ein Git-Repository für die Website einzurichten.

        azure site create mywebsite --git
3. Erstellen Sie ein neues Verzeichnis mit dem Namen **bin** unterhalb des Verzeichnisses, das die Datei **server.js** enthält.
4. Laden Sie die Version von **node.exe** (die Windows-Version) herunter, die Sie in Ihrer Anwendung verwenden möchten. Das folgende Beispiel verwendet **curl** , um Version 0.8.1 herunterzuladen:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Speichern Sie die Datei **node.exe** im zuvor erstellten Ordner **bin**.
5. Erstellen Sie eine Datei **iisnode.yml** im gleichen Verzeichnis wie die **server.js** und fügen Sie den folgenden Inhalt zur Datei **iisnode.yml** hinzu:

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Unter diesem Pfad befindet sich die Datei **node.exe** in Ihrem Projekt, sobald Sie Ihre Anwendung auf der Azure-Website veröffentlicht haben.
6. Veröffentlichen Sie Ihre Anwendung. Da die Website zuvor mit dem Parameter --git erstellt wurde, können Sie die Anwendungsdateien mit den folgenden Befehlen Ihrem lokalen Git-Repository hinzufügen und anschließend im Website-Repository veröffentlichen:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Öffnen Sie die Website nach Veröffentlichung der Anwendung in einem Browser. Sie sollten die folgende Nachricht sehen: "Hello from Azure running node version: v0.8.1".

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie gelernt haben, wie Sie die von ihrer Anwendung verwendete Node.js-Version angeben können, empfehlen wir die Themen [Arbeiten mit Modulen], [Erstellen und Bereitstellen einer Node.js-Website](app-service-web/web-sites-nodejs-develop-deploy-mac.md) und [Verwenden der Azure-Befehlszeilentools für Mac und Linux].

Weitere Informationen finden Sie im [Node.js Developer Center](/develop/nodejs/).

[Verwenden der Azure-Befehlszeilentools für Mac und Linux]: xplat-cli-install.md
[Azure-Befehlszeilentools]: xplat-cli-install.md
[Arbeiten mit Modulen]: nodejs-use-node-modules-azure-apps.md
[build and deploy a Node.js Web Site]: web-sites-nodejs-develop-deploy-mac.md



<!--HONumber=Dec16_HO2-->


