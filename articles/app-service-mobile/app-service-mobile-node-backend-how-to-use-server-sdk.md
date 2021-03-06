---
title: "Vorgehensweise beim Arbeiten mit dem Node.js Back-End Server SDK für Mobile Apps | Microsoft-Dokumentation"
description: "Informationen zum Arbeiten mit dem Node.js-Back-End-Server SDK für Azure Mobile App Service-Apps."
services: app-service\mobile
documentationcenter: 
author: adrianhall
manager: erikre
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 96683bc02cefb94e4252eac5364d1b9ff55cf3d4


---
# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Verwenden des Azure Mobile Apps SDK für Node.js
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Dieser Artikel enthält ausführliche Informationen und Beispiele, die veranschaulichen, wie Sie in Azure Mobile App Service-Apps ein Node.js-Back-End verwenden.

## <a name="a-nameintroductionaintroduction"></a><a name="Introduction"></a>Einführung
Mobile Azure App Service-Apps verfügen über eine Funktion zum Hinzufügen einer für Mobilgeräte optimierten Datenzugriff-Web-API zu einer Webanwendung.  Das Azure Mobile App Service-Apps SDK steht für ASP.NET- und Node.js-Webanwendungen zur Verfügung.  Das SDK ermöglicht die folgenden Vorgänge:

* Tabellenvorgänge (Lesen, Einfügen, Aktualisieren, Löschen) für den Datenzugriff
* Vorgänge der benutzerdefinierten API

Beide Vorgänge ermöglichen die Authentifizierung über alle Identitätsanbieter hinweg, die vom Azure App Service zugelassen werden, z. B. Identitätsanbieter per sozialem Netzwerk, wie Facebook, Twitter, Google und Microsoft, oder Azure Active Directory für die Unternehmensidentität.

Beispiele für die einzelnen Anwendungsfälle finden Sie im [Verzeichnis mit den Beispielen auf GitHub].

## <a name="supported-platforms"></a>Unterstützte Plattformen
Das Azure Mobile Apps Node SDK unterstützt das aktuelle LTS-Release von Node und höher.  Zum Zeitpunkt der Erstellung dieses Artikels ist die neueste LTS-Version Node v4.5.0.  Andere Node-Versionen funktionieren möglicherweise, werden aber nicht unterstützt.

Das Mobile Apps Node SDK unterstützt zwei Datenbanktreiber: Der node-mssql-Treiber unterstützt SQL Azure- und lokale SQL Server-Instanzen.  Der sqlite3-Treiber unterstützt SQLite-Datenbanken nur auf einer einzigen Instanz.

### <a name="a-namehowto-cmdline-basicappahow-to-create-a-basic-nodejs-backend-using-the-command-line"></a><a name="howto-cmdline-basicapp"></a>Vorgehensweise: Erstellen eines einfachen Node.js-Back-Ends über die Befehlszeile
Jedes Node.js-Back-End von Azure Mobile App Service-Apps verhält sich am Anfang wie eine ExpressJS-Anwendung.  ExpressJS ist das beliebteste Webdienst-Framework für Node.js.  Sie können eine [Express] -Basisanwendung wie folgt erstellen:

1. Erstellen Sie in einem Befehlsfenster oder PowerShell-Fenster ein Verzeichnis für Ihr Projekt.
   
        mkdir basicapp
2. Führen Sie „npm init“ aus, um die Paketstruktur zu initialisieren.
   
        cd basicapp
        npm init
   
    Im Rahmen des Befehls „npm init“ werden einige Fragen gestellt, um das Projekt initialisieren zu können.  Hier finden Sie eine Beispielausgabe:
   
    ![Ausgabe von npm init][0]
3. Installieren Sie die express- und azure-mobile-apps-Bibliotheken aus dem Repository „npm“.
   
        npm install --save express azure-mobile-apps
4. Erstellen Sie die Datei „app.js“, um den einfachen mobilen Server zu implementieren.
   
        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');
   
        var app = express(),
            mobile = azureMobileApps();
   
        // Define a TodoItem table
        mobile.tables.add('TodoItem');
   
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);
   
        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Mit dieser Anwendung wird eine für Mobilgeräte optimierte WebAPI mit einem einzelnen Endpunkt (`/tables/TodoItem`) erstellt, der über ein dynamisches Schema einen nicht authentifizierten Zugriff auf einen zugrunde liegenden SQL-Datenspeicher bereitstellt.  Mit der Anwendung können Sie die Schnellstarts der Clientbibliothek verfolgen:

* [Android-Client-Schnellstart]
* [Apache Cordova-Client-Schnellstart]
* [iOS-Client-Schnellstart]
* [Windows Store-Client-Schnellstart]
* [Xamarin.iOS-Client-Schnellstart]
* [Xamarin.Android-Client-Schnellstart]
* [Xamarin.Forms-Client-Schnellstart]

Den Code für diese einfache Anwendung finden Sie im [basicapp-Beispiel auf GitHub].

### <a name="a-namehowto-vs2015-basicappahow-to-create-a-node-backend-with-visual-studio-2015"></a><a name="howto-vs2015-basicapp"></a>Vorgehensweise: Erstellen eines Node.js-Back-Ends mit Visual Studio 2015
Für Visual Studio 2015 ist eine Erweiterung zum Entwickeln der Node.js-Anwendung in der IDE erforderlich.  Um zu beginnen, installieren Sie die [Node.js-Tools 1.1 für Visual Studio].  Erstellen Sie eine Express 4.x-Anwendung, nachdem die Installation der Node.js-Tools für Visual Studio abgeschlossen ist:

1. Öffnen Sie das Dialogfeld **Neues Projekt** (über **Datei** > **Neu** > **Projekt...**).
2. Erweitern Sie **Vorlagen** > **JavaScript** > **Node.js**.
3. Wählen Sie die **einfache Azure Node.js Express 4-Anwendung aus**.
4. Geben Sie den Projektnamen ein.  Klicken Sie auf *OK*.
   
    ![Visual Studio 2015 – Neues Projekt][1]
5. Klicken Sie mit der rechten Maustaste auf den Knoten **npm**, und wählen Sie **Neue NPM-Pakete installieren...**.
6. Bei der Erstellung der ersten Node.js-Anwendung müssen Sie den NPM-Katalog möglicherweise aktualisieren.  Klicken Sie bei Bedarf auf **Aktualisieren** .
7. Geben Sie in das Suchfeld *azure-mobile-apps* ein.  Klicken Sie auf das Paket **azure-mobile-apps 2.0.0** und dann auf **Paket installieren**.
   
    ![Neue NPM-Pakete installieren][2]
8. Klicken Sie auf **Schließen**.
9. Öffnen Sie die Datei *app.js*, um Unterstützung für das Azure Mobile Apps SDK-hinzuzufügen.  Fügen Sie unten in den „library require“-Anweisungen in Zeile 6 den folgenden Code hinzu:
   
        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');
   
    Fügen Sie ungefähr bei Zeile 27 nach den anderen „app.use“-Anweisungen den folgenden Code hinzu:
   
        app.use('/users', users);
   
        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);
   
    Speichern Sie die Datei .
10. Führen Sie die Anwendung entweder lokal aus (die API wird unter http://localhost:3000 bereitgestellt), oder veröffentlichen Sie sie in Azure.

### <a name="a-namecreate-node-backend-portalahow-to-create-a-nodejs-backend-using-the-azure-portal"></a><a name="create-node-backend-portal"></a>Vorgehensweise: Erstellen eines Node.js-Back-Ends mithilfe des Azure-Portals
Sie können ein Mobile App Back-End direkt im [Azure-Portal]erstellen. Sie können entweder die folgenden Schritte ausführen oder anhand der Anweisungen im Tutorial [Erstellen einer mobilen App](app-service-mobile-ios-get-started.md) einen Client und einen Server erstellen. Das Tutorial enthält eine vereinfachte Version dieser Anweisungen und eignet sich am besten für Proof of Concept-Projekte.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Wählen Sie auf dem Blatt *Erste Schritte* unter **Erstellen einer Tabellen-API** die Option **Node.js** als **Back-End-Sprache**. Aktivieren Sie die Schaltfläche **Ich bestätige, dass durch diese Aktion alle Websiteinhalte überschrieben werden**, und klicken Sie auf **TodoItem-Tabelle erstellen**.

### <a name="a-namedownload-quickstartahow-to-download-the-nodejs-backend-quickstart-code-project-using-git"></a><a name="download-quickstart"></a>Vorgehensweise: Herunterladen des Schnellstart-Codeprojekts für das Node.js-Back-End mit Git
Wenn Sie ein Node.js-Mobile App-Back-End erstellen, indem Sie im Portal das Blatt **Schnellstart** verwenden, wird ein Node.js-Projekt für Sie erstellt und auf Ihrer Website bereitgestellt. Im Portal können Sie Tabellen und APIs hinzufügen und Codedateien für das Node.js-Back-End bearbeiten. Sie können auch verschiedene Bereitstellungstools zum Herunterladen des Back-End-Projekts verwenden, damit Sie Tabellen und APIs hinzufügen oder ändern und das Projekt dann erneut veröffentlichen können. Weitere Informationen finden Sie im [Azure App Service-Bereitstellungshandbuch]. Beim folgenden Verfahren wird ein Git-Repository verwendet, um den Schnellstart-Projektcode herunterzuladen.

1. Installieren Sie Git, falls Sie dies noch nicht getan haben. Die erforderlichen Schritte zum Installieren von Git variieren je nach Betriebssystem. Informationen zu betriebssystemspezifischen Distributionen und zur Installation finden Sie unter [Installieren von Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) .
2. Führen Sie die Schritte unter [Aktivieren des App Service-App-Repositorys](../app-service-web/app-service-deploy-local-git.md#Step3) aus, um das Git-Repository für Ihre Back-End-Website zu aktivieren, und notieren Sie sich den Benutzernamen und das Kennwort der Bereitstellung.
3. Notieren Sie sich auf dem Blatt für das Mobile App-Back-End, die Einstellung unter **Git-Klon-URL** .
4. Führen Sie den Befehl `git clone` mit der Git-Klon-URL aus, und geben Sie Ihr Kennwort ein, wenn Sie dazu aufgefordert werden. Dies ist im folgenden Beispiel dargestellt:
   
        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Navigieren Sie zum lokalen Verzeichnis. Im obigen Beispiel ist dies „/todolist“. Sie sehen, dass Projektdateien heruntergeladen wurden. Suchen Sie die `todoitem.json`-Datei im `/tables`-Verzeichnis.  Diese Datei definiert Berechtigungen für die Tabelle.  Im gleichen Verzeichnis finden Sie auch die Datei `todoitem.js`, die diese CRUD-Vorgangsskripts für die Tabelle definiert.
6. Führen Sie nach dem Vornehmen der Änderungen an den Projektdateien die folgenden Befehle aus, um die Änderungen hinzuzufügen, dafür einen Commit auszuführen und sie dann auf die Website hochzuladen:
   
        $ git commit -m "updated the table script"
        $ git push origin master
   
    Wenn Sie dem Projekt neue Dateien hinzufügen, müssen Sie zuerst den Befehl `git add .` ausführen.

Die Website wird jeweils neu veröffentlicht, wenn ein neuer Satz mit Commits per Pushvorgang auf die Website übertragen wird.

### <a name="a-namehowto-publish-to-azureahow-to-publish-your-nodejs-backend-to-azure"></a><a name="howto-publish-to-azure"></a>Vorgehensweise: Veröffentlichen des Node.js-Back-Ends in Azure
Microsoft Azure bietet viele Verfahren zum Veröffentlichen Ihres Node.js-Back-Ends für Azure Mobile App Service-Apps im Azure-Dienst.  Hierzu gehören die Nutzung von Bereitstellungstools, die in Visual Studio integriert sind, Befehlszeilentools und Optionen für die fortlaufende Bereitstellung, die auf der Quellcodeverwaltung basieren.  Weitere Informationen zu diesem Thema finden Sie im [Azure App Service-Bereitstellungshandbuch].

Azure App Service bietet spezielle Hinweise für Node.js-Anwendungen, die Sie vor dem Bereitstellen lesen sollten:

* Vorgehensweise: [Angeben der Node.js-Version]
* Vorgehensweise: [Verwenden von Node.js-Modulen]

### <a name="a-namehowto-enable-homepageahow-to-enable-a-home-page-for-your-application"></a><a name="howto-enable-homepage"></a>Vorgehensweise: Aktivieren einer Startseite für Ihre Anwendung
Viele Anwendungen bestehen aus Web-Apps und mobilen Apps. Das ExpressJS-Framework ermöglicht Ihnen die Kombination dieser beiden App-Typen.  In manchen Fällen möchten Sie jedoch vielleicht nur eine Mobilschnittstelle implementieren.  Es ist hilfreich, eine Zielseite einzurichten, um sicherzustellen, dass der App-Dienst betriebsbereit ist.  Sie können entweder Ihre eigene Startseite bereitstellen oder eine temporäre Startseite aktivieren.  Um eine temporäre Startseite zu aktivieren, verwenden Sie folgenden Code zum Instanziieren von Azure Mobile Apps:

    var mobile = azureMobileApps({ homePage: true });

Wenn diese Option nur bei der lokalen Entwicklung zur Verfügung stehen soll, können Sie diese Einstellung der Datei `azureMobile.js` hinzufügen.

## <a name="a-nametableoperationsatable-operations"></a><a name="TableOperations"></a>Tabellenvorgänge
Das Node.js-Server SDK für „azure-mobile-apps“ bietet Verfahren zum Verfügbarmachen von Datentabellen, die in Azure SQL-Datenbank als WebAPI gespeichert sind.  Es werden fünf Vorgänge bereitgestellt.

| Vorgang | Beschreibung |
| --- | --- |
| GET /tables/*Tabellenname* |Alle Datensätze der Tabelle abrufen |
| GET /tables/*Tabellenname*/:id |Bestimmten Datensatz der Tabelle abrufen |
| POST /tables/*Tabellenname* |Datensatz in der Tabelle erstellen |
| PATCH /tables/*Tabellenname*/:id |Datensatz in der Tabelle aktualisieren |
| DELETE /tables/*Tabellenname*/:id |Datensatz in der Tabelle löschen |

Diese WebAPI unterstützt [OData] und erweitert das Tabellenschema, um die [Synchronisierung von Offlinedaten] zu unterstützen.

### <a name="a-namehowto-dynamicschemaahow-to-define-tables-using-a-dynamic-schema"></a><a name="howto-dynamicschema"></a>Vorgehensweise: Definieren von Tabellen mit einem dynamischen Schema
Bevor eine Tabelle verwendet werden kann, muss sie definiert werden.  Tabellen können mit einem statischen Schema (der Entwickler definiert die Spalten im Schema) oder dynamisch (das SDK steuert das Schema basierend auf eingehenden Anforderungen) definiert werden. Außerdem kann der Entwickler bestimmte Aspekte der WebAPI steuern, indem er der Definition JavaScript-Code hinzufügt.

Die bewährte Methode hierbei lautet: Definieren Sie jede Tabelle in einer JavaScript-Datei im Verzeichnis „tables“, und verwenden Sie dann die tables.import()-Methode, um die Tabellen zu importieren.  Zum Erweitern der einfachen App (basic-app) wird die Datei „app.js“ angepasst:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definieren Sie die Tabelle in „./tables/TodoItem.js“:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Für Tabellen werden standardmäßig dynamische Schemas verwendet.  Zum globalen Deaktivieren des dynamischen Schemas legen Sie die App-Einstellung **MS_DynamicSchema** im Azure-Portal auf „false“ fest.

Ein vollständiges Beispiel finden Sie im [todo-Beispiel auf GitHub].

### <a name="a-namehowto-staticschemaahow-to-define-tables-using-a-static-schema"></a><a name="howto-staticschema"></a>Vorgehensweise: Definieren von Tabellen mit einem statischen Schema
Sie können die Spalten explizit definieren, die über die WebAPI verfügbar gemacht werden sollen.  Das Node.js SDK für „azure-mobile-apps“ fügt der von Ihnen bereitgestellten Liste automatisch alle zusätzlichen Spalten hinzu, die für die Synchronisierung von Offlinedaten benötigt werden.  Für die Schnellstart-Clientanwendungen ist beispielsweise eine Tabelle mit zwei Spalten erforderlich: „text“ (eine Zeichenfolge) und „complete“ (boolescher Wert).  
Die Tabelle kann wie folgt in der JavaScript-Datei mit der Tabellendefinition (im Verzeichnis „tables“) festgelegt werden:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Wenn Sie Tabellen statisch definieren, müssen Sie auch die tables.initialize()-Methode aufrufen, um das Datenbankschema beim Starten zu erstellen.  Die Methode „tables.initialize()“ gibt eine [Zusage] (Promise) zurück. Hiermit wird sichergestellt, dass der Webdienst erst dann Anforderungen verarbeitet, nachdem die Datenbank initialisiert wurde.

### <a name="a-namehowto-sqlexpress-setupahow-to-use-sql-express-as-a-development-data-store-on-your-local-machine"></a><a name="howto-sqlexpress-setup"></a>Vorgehensweise: Verwenden von SQL Express als Entwicklungsdatenspeicher auf Ihrem lokalen Computer
Das Node.js SDK für Azure Mobile Apps bietet drei standardmäßige Optionen zum Bereitstellen von Daten:

* Verwenden des **memory** -Treibers zum Bereitstellen eines nicht beständigen Beispielspeichers
* Verwenden des **mssql** -Treibers zum Bereitstellen eines SQL Express-Datenspeichers für die Entwicklung
* Verwenden des **mssql** -Treibers zum Bereitstellen eines Azure SQL-Datenbank-Datenspeichers für die Produktion

Das Node.js SDK für Azure Mobile Apps verwendet das [mssql-Node.js-Paket] , um eine Verbindung mit SQL Express und SQL-Datenbank einzurichten und zu nutzen.  Für dieses Paket müssen Sie TCP-Verbindungen auf Ihrer SQL Express-Instanz aktivieren.

> [!TIP]
> Beim memory-Treiber wird keine vollständige Gruppe von Elementen für Testzwecke bereitgestellt.  Wenn Sie Ihr Back-End lokal testen möchten, empfehlen wir die Verwendung eines SQL Express-Datenspeichers und des mssql-Treibers.
> 
> 

1. Laden Sie [Microsoft SQL Server 2014 Express]herunter, und installieren Sie die Anwendung.  Stellen Sie sicher, dass Sie die Edition „SQL Server 2014 Express with Tools“ installieren.  Falls Sie nicht unbedingt 64-Bit-Unterstützung benötigen, können Sie die 32-Bit-Version verwenden, die bei der Ausführung weniger Arbeitsspeicher verbraucht.
2. Führen Sie den SQL Server 2014-Konfigurations-Manager aus.
   
   1. Erweitern Sie im linken Strukturmenü den Knoten **SQL Server-Netzwerkkonfiguration** .
   2. Klicken Sie auf **Protokolle für SQLEXPRESS**.
   3. Klicken Sie mit der rechten Maustaste auf **TCP/IP**, und wählen Sie **Aktivieren**.  Klicken Sie im Popupdialogfenster auf **OK** .
   4. Klicken Sie mit der rechten Maustaste auf **TCP/IP**, und wählen Sie **Eigenschaften**.
   5. Klicken Sie auf die Registerkarte **IP-Adressen** .
   6. Suchen Sie nach dem Knoten **IPAll** .  Geben Sie im Feld **TCP-Port** den Wert **1433** ein.
      
          ![Configure SQL Express for TCP/IP][3]
   7. Klicken Sie auf **OK**.  Klicken Sie im Popupdialogfenster auf **OK** .
   8. Klicken Sie im linken Strukturmenü auf **SQL Server-Dienste** .
   9. Klicken Sie mit der rechten Maustaste auf **SQL Server (SQLEXPRESS)**, und wählen Sie **Neu starten**.
   10. Schließen Sie den SQL Server 2014-Konfigurations-Manager.
3. Ausführen von SQL Server 2014 Management Studio und Herstellen einer Verbindung mit Ihrer lokalen SQL Express-Instanz
   
   1. Klicken Sie mit der rechten Maustaste im Objekt-Explorer auf Ihre Instanz, und wählen Sie **Eigenschaften**
   2. Wählen Sie die Seite **Sicherheit** aus.
   3. Stellen Sie sicher, dass der **SQL Server- und Windows-Authentifizierungsmodus** ausgewählt ist.
   4. Klicken Sie auf **OK**
      
          ![Configure SQL Express Authentication][4]
   5. Erweitern Sie **Sicherheit** > **Anmeldungen** .
   6. Klicken Sie mit der rechten Maustaste auf **Anmeldungen**, und wählen Sie **Neue Anmeldung...**.
   7. Geben Sie einen Anmeldenamen ein.  Wählen Sie **SQL Server-Authentifizierung**.  Geben Sie ein Kennwort ein, und geben Sie das gleiche Kennwort dann noch einmal unter **Kennwort bestätigen** ein.  Das Kennwort muss die unter Windows erforderliche Kennwortkomplexität aufweisen.
   8. Klicken Sie auf **OK**
      
          ![Add a new user to SQL Express][5]
   9. Klicken Sie mit der rechten Maustaste auf die neue Anmeldung, und wählen Sie **Eigenschaften**
   10. Wählen Sie die Seite **Serverrollen** aus.
   11. Aktivieren Sie das Kontrollkästchen neben der Serverrolle **dbcreator** .
   12. Klicken Sie auf **OK**
   13. Schließen Sie SQL Server 2015 Management Studio.

Notieren Sie sich den gewählten Benutzernamen und das gewählte Kennwort.  Unter Umständen müssen Sie je nach Ihren speziellen Datenbankanforderungen weitere Serverrollen oder Berechtigungen zuweisen.

Die Node.js-Anwendung liest die Umgebungsvariable **SQLCONNSTR_MS_TableConnectionString**, um die Verbindungszeichenfolge für diese Datenbank abzurufen.  Sie können diese Variable in Ihrer Umgebung festlegen.  Beispielsweise können Sie PowerShell verwenden, um diese Umgebungsvariable festzulegen:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Greifen Sie über eine TCP/IP-Verbindung auf die Datenbank zu, und geben Sie einen Benutzernamen und ein Kennwort für die Verbindung an.

### <a name="a-namehowto-config-localdevahow-to-configure-your-project-for-local-development"></a><a name="howto-config-localdev"></a>Vorgehensweise: Konfigurieren des Projekts für die lokale Entwicklung
Azure Mobile Apps liest eine JavaScript-Datei mit dem Namen *azureMobile.js* aus dem lokalen Dateisystem.  Verwenden Sie diese Datei nicht, um das Azure Mobile Apps SDK in der Produktion zu konfigurieren. Verwenden Sie stattdessen die App-Einstellungen im [Azure-Portal].  Von der Datei *azureMobile.js* muss ein Konfigurationsobjekt exportiert werden.  Die am häufigsten verwendeten Einstellungen lauten:

* Datenbankeinstellungen
* Diagnoseprotokollierungseinstellungen
* Alternative CORS-Einstellungen

Hier finden Sie eine Beispieldatei für *azureMobile.js* , mit der die oben beschriebenen Datenbankeinstellungen implementiert werden:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Es ist ratsam, dass Sie *azureMobile.js* Ihrer *.gitignore*-Datei hinzufügen (oder eine andere Datei zum Ignorieren der Quellcodeverwaltung), um zu verhindern, dass Kennwörter in der Cloud gespeichert werden.  Konfigurieren Sie die Produktionseinstellungen in den App-Einstellungen immer im [Azure-Portal].

### <a name="a-namehowto-appsettingsahow-configure-app-settings-for-your-mobile-app"></a><a name="howto-appsettings"></a>Vorgehensweise: Konfigurieren von App-Einstellungen für Ihre mobile App
Für die meisten Einstellungen in der Datei *azureMobile.js* gibt es im [Azure-Portal]eine entsprechende App-Einstellung.  Verwenden Sie die folgende Liste, um Ihre App unter „App-Einstellungen“ zu konfigurieren:

| App-Einstellung | *azureMobile.js* -Einstellung | Beschreibung | Gültige Werte |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |Name |Der Name der App. |string |
| **MS_MobileLoggingLevel** |logging.level |Mindestprotokolliergrad für die zu protokollierenden Meldungen. |error, warning, info, verbose, debug, silly |
| **MS_DebugMode** |debug |Aktivieren oder Deaktivieren des Debugmodus. |true, false |
| **MS_TableSchema** |data.schema |Name des Standardschemas für SQL-Tabellen |string (default: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Aktivieren oder Deaktivieren des Debugmodus. |true, false |
| **MS_DisableVersionHeader** |version (set to undefined) |Deaktiviert den Header „X-ZUMO-Server-Version“. |true, false |
| **MS_SkipVersionCheck** |skipversioncheck |Deaktiviert die Überprüfung der Client-API-Version. |true, false |

So legen Sie eine App-Einstellung fest:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Alle Ressourcen** oder **App Services** aus, und klicken Sie dann auf den Namen Ihrer mobilen App.
3. Das Blatt „Einstellungen“ wird standardmäßig geöffnet. Wenn dies nicht der Fall ist, klicken Sie auf **Einstellungen**.
4. Klicken Sie im Menü „Allgemein“ auf **Anwendungseinstellungen** .
5. Scrollen Sie zum Abschnitt „App-Einstellungen“.
6. Wenn Ihre App-Einstellung bereits vorhanden ist, klicken Sie auf den Wert der App-Einstellung, um ihn zu bearbeiten.
7. Wenn die App-Einstellung nicht vorhanden ist, geben Sie im Feld „Schlüssel“ die App-Einstellung und im Feld „Wert“ den Wert ein.
8. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

Nach dem Ändern der meisten App-Einstellungen ist ein Neustart des Diensts erforderlich.

### <a name="a-namehowto-use-sqlazureahow-to-use-sql-database-as-your-production-data-store"></a><a name="howto-use-sqlazure"></a>Vorgehensweise: Verwenden von SQL-Datenbank als Datenspeicher für die Produktion
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Das Verwenden von Azure SQL-Datenbank als Datenspeicher ist über alle Azure App Service-Anwendungstypen hinweg identisch. Führen Sie diese Schritte zum Erstellen eines Mobile App-Back-Ends aus, falls Sie dies noch nicht getan haben.

1. Melden Sie sich beim [Azure-Portal]an.
2. Klicken Sie oben links im Fenster auf **+NEU** > **Web und mobil** > **Mobile App**, und geben Sie dann einen Namen für Ihr mobiles App-Back-End an.
3. Geben Sie im Feld **Ressourcengruppe** den gleichen Namen wie für Ihre App ein.
4. Es wird der App Service-Plan "Standard" ausgewählt.  Wenn Sie Ihren App Service-Plan ändern möchten, klicken Sie auf App Service-Plan > **+ Neu erstellen**.  Geben Sie einen Namen für den neuen App Service-Tarif ein, und wählen Sie einen geeigneten Speicherort.  Klicken Sie auf den Tarif, und wählen Sie einen geeigneten Tarif für den Dienst. Wählen Sie **Alle anzeigen** aus, um mehr Tarifoptionen anzuzeigen, z. B. **Free** und **Shared**.  Nachdem Sie den Tarif ausgewählt haben, klicken Sie auf die Schaltfläche **Auswählen**.  Klicken Sie wieder auf dem Blatt **App Service-Plan** auf **OK**.
5. Klicken Sie auf **Erstellen**. Das Bereitstellen eines mobilen App-Back-Ends kann einige Minuten in Anspruch nehmen.  Nachdem das Mobile App-Back-End bereitgestellt wurde, wird im Portal das Blatt **Einstellungen** für das Mobile App-Back-End geöffnet.

Nach der Erstellung des mobilen App-Back-Ends können Sie wählen, ob Sie entweder für eine vorhandene SQL-Datenbank eine Verbindung mit Ihrem mobilen App-Back-End herstellen oder eine neue SQL-Datenbank erstellen.  In diesem Abschnitt erstellen wir eine SQL-Datenbank.

> [!NOTE]
> Wenn Sie am Standort des mobilen App-Back-Ends bereits über eine Datenbank verfügen, können Sie stattdessen **Vorhandene Datenbank verwenden** wählen und dann diese Datenbank auswählen. Die Verwendung einer Datenbank an einem anderen Standort wird aufgrund der höheren Latenz nicht empfohlen.
> 
> 

1. Klicken Sie im neuen mobilen App-Back-End auf **Einstellungen** > **Mobile App** > **Daten** > **+Hinzufügen**.
2. Klicken Sie auf dem Blatt **Datenverbindung hinzufügen** auf **SQL-Datenbank – Erforderliche Einstellungen konfigurieren** > **Neue Datenbank erstellen**.  Geben Sie den Namen der neuen Datenbank in das Feld **Name** ein.
3. Klicken Sie auf **Server**.  Geben Sie auf dem Blatt **Neuer Server** in **Servername** einen eindeutigen Namen ein, und legen Sie eine geeignete **Serveradministratoranmeldung** mit einem **Kennwort** an.  Stellen Sie sicher, dass **Allow azure services to access server** aktiviert ist.  Klicken Sie auf **OK**.
   
    ![Erstellen einer Azure-SQL-Datenbank][6]
4. Klicken Sie auf dem Blatt **Neue Datenbank** auf **OK**.
5. Wählen Sie wieder auf dem Blatt **Datenverbindung hinzufügen** die **Verbindungszeichenfolge** aus, und geben Sie den Anmeldenamen und das Kennwort ein, den/das Sie beim Erstellen der Datenbank angegeben haben.  Wenn Sie eine vorhandene Datenbank verwenden, geben Sie die Anmeldeinformationen für diese Datenbank an.  Klicken Sie nach der Eingabe auf **OK**.
6. Klicken Sie auf dem Blatt **Datenverbindung hinzufügen** auf **OK**, um die Datenbank zu erstellen.

<!--- END OF ALTERNATE INCLUDE -->

Das Erstellen der Datenbank kann einige Minuten dauern.  Im Bereich **Benachrichtigungen** können Sie den Fortschritt der Bereitstellung überwachen.  Fahren Sie erst fort, wenn die Datenbank erfolgreich bereitgestellt wurde.  Nach der erfolgreichen Bereitstellung wird in den Einstellungen des Mobile App-Back-Ends eine Verbindungszeichenfolge für die SQL-Datenbankinstanz erstellt.  Sie können diese App-Einstellung unter **Einstellungen** > **Anwendungseinstellungen** > **Verbindungszeichenfolgen**.

### <a name="a-namehowto-tables-authahow-to-require-authentication-for-access-to-tables"></a><a name="howto-tables-auth"></a>Vorgehensweise: Erzwingen der Authentifizierung für den Zugriff auf Tabellen
Wenn Sie die App Service-Authentifizierung mit dem tables-Endpunkt verwenden möchten, müssen Sie die App Service-Authentifizierung zuerst im [Azure-Portal] konfigurieren.  Weitere Informationen zum Konfigurieren der Authentifizierung in einem Azure App Service finden Sie im Konfigurationshandbuch für den Identitätsanbieter, den Sie verwenden möchten:

* [So wird‘s gemacht: Konfigurieren der Azure Active Directory-Authentifizierung]
* [So wird‘s gemacht: Konfigurieren der Facebook-Authentifizierung]
* [So wird‘s gemacht: Konfigurieren der Google-Authentifizierung]
* [So wird‘s gemacht: Konfigurieren der Microsoft-Authentifizierung]
* [So wird‘s gemacht: Konfigurieren der Twitter-Authentifizierung]

Jede Tabelle verfügt über eine access-Eigenschaft, die zum Steuern des Zugriffs auf die Tabelle verwendet werden kann.  Das folgende Beispiel zeigt eine statisch definierte Tabelle mit erforderlicher Authentifizierung.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Die „access“-Eigenschaft kann einen von drei Werten haben:

* *anonymous* gibt an, dass die Clientanwendung Daten ohne Authentifizierung lesen darf.
* *authenticated* gibt an, dass die Clientanwendung zusammen mit der Anforderung ein gültiges Authentifizierungstoken senden muss.
* *disabled* gibt an, dass die Tabelle derzeit deaktiviert ist.

Wenn die access-Eigenschaft nicht definiert ist, ist der Zugriff ohne Authentifizierung zulässig.

### <a name="a-namehowto-tables-getidentityahow-to-use-authentication-claims-with-your-tables"></a><a name="howto-tables-getidentity"></a>Vorgehensweise: Verwenden von Authentifizierungsansprüchen für Tabellen
Sie können verschiedene Ansprüche einrichten, die beim Einrichten der Authentifizierung angefordert werden.  Diese Ansprüche stehen normalerweise nicht über das `context.user` -Objekt zur Verfügung.  Mit der `context.user.getIdentity()` -Methode können sie jedoch abgerufen werden.  Die `getIdentity()` -Methode gibt eine Zusage zurück, die in ein Objekt aufgelöst wird.  Das Objekt wird durch die Authentifizierungsmethode („facebook“, „google“, „twitter“, „microsoftaccount“ oder „aad“) ermittelt.

Wenn Sie beispielsweise die Microsoft Account-Authentifizierung einrichten und den Anspruch „E-Mail-Adressen“ anfordern, lässt sich die E-Mail-Adresse mit dem folgenden Tabellencontroller zum Datensatz hinzufügen:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Um zu sehen, welche Ansprüche verfügbar sind, verwenden Sie einen Webbrowser zum Anzeigen des `/.auth/me` -Endpunkts der Website.

### <a name="a-namehowto-tables-disabledahow-to-disable-access-to-specific-table-operations"></a><a name="howto-tables-disabled"></a>Vorgehensweise: Deaktivieren des Zugriffs auf bestimmte Tabellenvorgänge
Die access-Eigenschaft kann nicht nur in der Tabelle angezeigt werden, sondern sie kann auch verwendet werden, um einzelne Vorgänge zu steuern.  Es gibt vier Vorgänge:

* *read* ist der auf die Tabelle angewendete RESTful GET-Vorgang.
* *insert* ist der auf die Tabelle angewendete RESTful POST-Vorgang.
* *update* ist der auf die Tabelle angewendete RESTful PATCH-Vorgang.
* *delete* ist der auf die Tabelle angewendete RESTful DELETE-Vorgang.

Es kann beispielsweise sein, dass Sie eine schreibgeschützte nicht authentifizierte Tabelle bereitstellen möchten:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="a-namehowto-tables-queryahow-to-adjust-the-query-that-is-used-with-table-operations"></a><a name="howto-tables-query"></a>Vorgehensweise: Anpassen der Abfrage für Tabellenvorgänge
Eine häufige Anforderung an Tabellenvorgänge ist das Bereitstellen einer eingeschränkten Anzeige von Daten.  Beispielsweise können Sie eine Tabelle bereitstellen, die mit der authentifizierten Benutzer-ID gekennzeichnet ist, damit Sie nur Ihre eigenen Datensätze lesen oder aktualisieren können.  Mit der folgenden Tabellendefinition werden diese Funktionen bereitgestellt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Vorgänge, bei denen normalerweise eine Abfrage ausgeführt wird, verfügen über eine Abfrageeigenschaft, die Sie mit einer where-Klausel anpassen können. Die Abfrageeigenschaft ist ein [QueryJS] -Objekt zum Konvertieren einer OData-Abfrage in ein Element, das vom Daten-Back-End verarbeitet werden kann.  Für einfache Gleichheit (wie im vorherigen Fall) kann eine Zuordnung verwendet werden. Sie können auch bestimmte SQL-Klauseln hinzufügen:

    context.query.where('myfield eq ?', 'value');

### <a name="a-namehowto-tables-softdeleteahow-to-configure-soft-delete-on-a-table"></a><a name="howto-tables-softdelete"></a>Vorgehensweise: Konfigurieren des vorläufigen Löschens in einer Tabelle
Beim vorläufigen Löschen werden Datensätze nicht tatsächlich gelöscht.  Stattdessen werden sie in der Datenbank als gelöscht markiert, indem die Spalte „deleted“ auf „true“ festgelegt wird.  Das Azure Mobile Apps-SDK entfernt vorläufig gelöschte Datensätze automatisch aus Ergebnissen, es sei denn, im Mobile Client-SDK wird „IncludeDeleted()“ verwendet.  Legen Sie in der Tabellendefinitionsdatei die `softDelete` -Eigenschaft fest, um eine Tabelle für das vorläufige Löschen zu konfigurieren:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Sie sollten ein Verfahren zum Bereinigen von Datensätzen einrichten – entweder über eine Clientanwendung, einen WebJob, eine Azure Function oder eine benutzerdefinierte API.

### <a name="a-namehowto-tables-seedingahow-to-seed-your-database-with-data"></a><a name="howto-tables-seeding"></a>Vorgehensweise: Durchführen des Seedings für Ihre Datenbank mit Daten
Beim Erstellen einer neuen Anwendung kann es sein, dass Sie für eine Tabelle das Seeding mit Daten durchführen möchten.  Sie können dies in der JavaScript-Datei mit der Tabellendefinition wie folgt durchführen:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Das Seeding von Daten wird nur durchgeführt, wenn die Tabelle mit dem Azure Mobile Apps SDK erstellt wird.  Falls die Tabelle in der Datenbank bereits vorhanden ist, werden keine Daten in die Tabelle eingefügt.  Wenn das dynamische Schema aktiviert ist, wird das Schema aus den Seedingdaten abgeleitet.

Wir empfehlen, die `tables.initialize()` -Methode explizit aufzurufen, um die Tabelle zu erstellen, wenn die Ausführung des Diensts beginnt.

### <a name="a-nameswaggerahow-to-enable-swagger-support"></a><a name="Swagger"></a>Vorgehensweise: Aktivieren der Swagger-Unterstützung
[Swagger] -Unterstützung ist in Azure Mobile App Service-Apps bereits integriert.  Um die Swagger-Unterstützung zu aktivieren, installieren Sie zuerst die Swagger-Benutzeroberfläche als Abhängigkeit:

    npm install --save swagger-ui

Nach der Installation können Sie die Swagger-Unterstützung im Azure Mobile Apps-Konstruktor aktivieren:

    var mobile = azureMobileApps({ swagger: true });

Die Swagger-Unterstützung soll wahrscheinlich nur in Entwicklungseditionen aktiviert werden.  Hierzu können Sie die App-Einstellung `NODE_ENV` verwenden:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Der Swagger-Endpunkt befindet sich unter http://*yoursite*.azurewebsites.net/swagger.  Sie können über den `/swagger/ui` -Endpunkt auf die Swagger-Benutzeroberfläche zugreifen.  Wenn Sie die Authentifizierung in Ihrer gesamten Anwendung als erforderlich festlegen, gibt Swagger einen Fehler aus.  Um optimale Ergebnisse zu erzielen, lassen Sie in den Einstellungen für die Azure App Service-Authentifizierung/-Autorisierung nicht authentifizierte Anforderungen zu, und steuern Sie dann die Authentifizierung mithilfe der `table.access` -Eigenschaft.

Sie können die Swagger-Option auch der Datei `azureMobile.js` hinzufügen, wenn die Swagger-Unterstützung nur bei der lokalen Entwicklung verfügbar sein soll.

## <a name="a-namepushpush-notifications"></a><a name="push">Pushbenachrichtigungen
Mobile Apps ist in Azure Notification Hubs integriert, damit Sie gezielte Pushbenachrichtigungen an Millionen von Geräten auf allen gängigen Plattformen senden können. Mithilfe von Benachrichtigungshubs können Sie Pushbenachrichtigungen an iOS-, Android- und Windows-Geräte senden. Weitere Informationen zu allem, was mit Notification Hubs machbar ist, finden Sie unter [Übersicht über Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

### <a name="aa-namesend-pushahow-to-send-push-notifications"></a></a><a name="send-push"></a>Vorgehensweise: Senden von Pushbenachrichtigungen
Der folgende Code zeigt, wie Sie das Pushobjekt zum Senden einer Pushbenachrichtigung an registrierte iOS-Geräte verwenden:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Indem Sie auf dem Client eine Pushregistrierungsvorlage erstellen, können Sie stattdessen eine Pushnachrichtenvorlage an Geräte auf allen unterstützten Plattformen senden. Der folgende Code zeigt, wie eine Benachrichtigungsvorlage gesendet wird:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <a name="a-namepush-userahow-to-send-push-notifications-to-an-authenticated-user-using-tags"></a><a name="push-user"></a>Vorgehensweise: Senden von Pushbenachrichtigungen an einen authentifizierten Benutzer mithilfe von Tags
Wenn ein authentifizierter Benutzer für Pushbenachrichtigungen registriert wird, wird der Registrierung automatisch ein Tag mit der Benutzer-ID hinzugefügt. Mithilfe dieses Tags können Sie Pushbenachrichtigungen an alle Geräte senden, die von einem bestimmten Benutzer registriert wurden. Mit dem folgenden Code wird die SID des Benutzers abgerufen, der die Anforderung stellt, und an jede Geräteregistrierung für diesen Benutzer wird eine Pushbenachrichtigungsvorlage gesendet:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Stellen Sie vor der Registrierung für Pushbenachrichtigungen von einem authentifizierten Client sicher, dass die Authentifizierung abgeschlossen ist.

## <a name="a-namecustomapia-custom-apis"></a><a name="CustomAPI"></a> Benutzerdefinierte APIs
### <a name="a-namehowto-customapi-basicahow-to-define-a-custom-api"></a><a name="howto-customapi-basic"></a>Gewusst wie: Definieren einer benutzerdefinierten API
Zusätzlich zur Datenzugriff-API über den /tables-Endpunkt können Azure Mobile Apps auch eine benutzerdefinierte API-Abdeckung bereitstellen.  Benutzerdefinierte APIs werden auf ähnliche Weise wie Tabellendefinitionen definiert und können auf die gleichen Funktionen zugreifen, einschließlich der Authentifizierung.

Wenn Sie die App Service-Authentifizierung mit einer benutzerdefinierten API verwenden möchten, müssen Sie die App Service-Authentifizierung zuerst im [Azure-Portal] konfigurieren.  Weitere Informationen zum Konfigurieren der Authentifizierung in einem Azure App Service finden Sie im Konfigurationshandbuch für den Identitätsanbieter, den Sie verwenden möchten:

* [So wird‘s gemacht: Konfigurieren der Azure Active Directory-Authentifizierung]
* [So wird‘s gemacht: Konfigurieren der Facebook-Authentifizierung]
* [So wird‘s gemacht: Konfigurieren der Google-Authentifizierung]
* [So wird‘s gemacht: Konfigurieren der Microsoft-Authentifizierung]
* [So wird‘s gemacht: Konfigurieren der Twitter-Authentifizierung]

Benutzerdefinierte APIs werden auf ähnliche Weise wie die Tabellen-API definiert.

1. Erstellen Sie ein **api** -Verzeichnis.
2. Erstellen Sie im **api** -Verzeichnis eine JavaScript-Datei mit einer API-Definition.
3. Verwenden Sie die „import“-Methode, um das Verzeichnis **api** zu importieren.

Hier ist die API-Prototypdefinition angegeben, die auf dem bereits verwendeten basic-app-Beispiel basiert.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Wir verwenden eine Beispiel-API, die mithilfe der *Date.now()* -Methode das Serverdatum zurückgibt.  Dies ist die Datei „api/date.js“:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Jeder Parameter entspricht einem der standardmäßigen RESTful-Verben: GET, POST, PATCH oder DELETE.  Bei der Methode handelt es sich um eine [ExpressJS Middleware] -Standardfunktion, mit der die erforderliche Ausgabe gesendet wird.

### <a name="a-namehowto-customapi-authahow-to-require-authentication-for-access-to-a-custom-api"></a><a name="howto-customapi-auth"></a>Vorgehensweise: Erzwingen der Authentifizierung für den Zugriff auf eine benutzerdefinierte API
Das Azure Mobile Apps-SDK implementiert die Authentifizierung sowohl für den tables-Endpunkt als auch für benutzerdefinierte APIs auf die gleiche Weise.  Fügen Sie eine **access** -Eigenschaft hinzu, um die Authentifizierung für die im vorherigen Abschnitt entwickelte API hinzuzufügen:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Sie können die Authentifizierung auch für bestimmte Vorgänge angeben:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Dasselbe Token, das für den tables-Endpunkt verwendet wird, muss für benutzerdefinierte APIs verwendet werden, die eine Authentifizierung benötigen.

### <a name="a-namehowto-customapi-authahow-to-handle-large-file-uploads"></a><a name="howto-customapi-auth"></a>Vorgehensweise: Ausführen großer Dateiuploads
Das Azure Mobile Apps-SDK verwendet die [body-parser-Middleware](https://github.com/expressjs/body-parser) , um Textinhalt in Ihrer Übermittlung zu akzeptieren und zu decodieren.  Sie können den Body-Parser im Voraus so konfigurieren, dass größere Dateiuploads akzeptiert werden:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Diese Datei wird vor der Übertragung Base64-codiert.  Dadurch erhöht sich die Größe des tatsächlichen Uploads (und damit auch die Größe, die Sie einberechnen müssen).

### <a name="a-namehowto-customapi-sqlahow-to-execute-custom-sql-statements"></a><a name="howto-customapi-sql"></a>Vorgehensweise: Ausführen von benutzerdefinierten SQL-Anweisungen
Das Azure Mobile Apps-SDK ermöglicht über das Anforderungsobjekt Zugriff auf den gesamten Kontext, sodass Sie problemlose parametrisierte SQL-Anweisungen für den definierten Datenanbieter ausführen können.

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="a-namedebuggingadebugging-easy-tables-and-easy-apis"></a><a name="Debugging"></a>Debuggen, Easy Tables und Easy APIs
### <a name="a-namehowto-diagnostic-logsahow-to-debug-diagnose-and-troubleshoot-azure-mobile-apps"></a><a name="howto-diagnostic-logs"></a>Debuggen, Diagnose und Problembehandlung bei Azure Mobile Apps
Der Azure App Service stellt mehrere Debugging- und Problembehandlungsverfahren für Node.js-Anwendungen bereit.
Folgende Artikel helfen Ihnen beim Einstieg in die Problembehandlung Ihres Node.js Mobile-Back-Ends:

* [Überwachen eines Azure App Service]
* [Aktivieren der Diagnoseprotokollierung in Azure App Service]
* [Problembehandlung für einen Azure App Service in Visual Studio]

Node.js-Anwendungen haben Zugriff auf viele Tools für die Diagnoseprotokollierung.  Intern nutzt das Azure Mobile Apps Node.js SDK [Winston] für die Diagnoseprotokollierung.  Die Protokollierung wird automatisch aktiviert, indem der Debugmodus aktiviert oder die App-Einstellung **MS_DebugMode** im [Azure-Portal] auf „true“ festgelegt wird. Generierte Protokolle werden im [Azure-Portal]in den Diagnoseprotokollen angezeigt.

### <a name="a-namein-portal-editingaa-namework-easy-tablesahow-to-work-with-easy-tables-in-the-azure-portal"></a><a name="in-portal-editing"></a><a name="work-easy-tables"></a>Vorgehensweise: Verwenden von Easy Tables im Azure-Portal
Mit Easy Tables können Sie direkt im Portal Tabellen erstellen und verwenden. Sie können mit dem App Service-Editor sogar Tabellenvorgänge bearbeiten.

Wenn Sie in den Einstellungen für die Back-End-Website auf **Easy Tables** klicken, können Sie eine Tabelle hinzufügen, ändern oder löschen. Sie sehen auch, dass Daten in der Tabelle enthalten sind.

![Verwenden von Easy Tables](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Die folgenden Befehle stehen in der Befehlsleiste einer Tabelle zur Verfügung:

* **Berechtigungen ändern** : Dient zum Ändern der Berechtigung für Vorgänge zum Lesen, Einfügen, Aktualisieren und Löschen in der Tabelle. 
  Sie können den anonymen Zugriff zulassen, eine Authentifizierung erzwingen oder den gesamten Zugriff auf den Vorgang deaktivieren. 
* **Skript bearbeiten** : Die Skriptdatei für die Tabelle wird im App Service-Editor geöffnet.
* **Schema verwalten** : Dient zum Hinzufügen oder Löschen von Spalten oder zum Ändern des Tabellenindex.
* **Tabelle löschen** : Dient zum Abschneiden einer vorhandenen Tabelle, indem alle Datenzeilen gelöscht werden, während das Schema unverändert bleibt.
* **Zeilen löschen** : Dient zum Löschen einzelner Zeilen mit Daten.
* **Streamingprotokolle anzeigen** : Dient zum Herstellen einer Verbindung mit dem Streamingprotokolldienst für Ihre Website.

### <a name="a-namework-easy-apisahow-to-work-with-easy-apis-in-the-azure-portal"></a><a name="work-easy-apis"></a>Vorgehensweise: Verwenden von Easy APIs im Azure-Portal
Mit Easy APIs können Sie direkt im Portal benutzerdefinierte APIs erstellen und verwenden. Sie können mit dem App Service-Editor API-Skripts bearbeiten.

Wenn Sie in den Einstellungen für die Back-End-Website auf **Easy APIs** klicken, können Sie einen benutzerdefinierten API-Endpunkt hinzufügen, ändern oder löschen.

![Verwenden von Easy APIs](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Im Portal können Sie die Zugriffsberechtigungen für eine bestimmte HTTP-Aktion ändern, die API-Skriptdatei im App Service-Editor bearbeiten oder die Streamingprotokolle anzeigen.

### <a name="a-nameonline-editorahow-to-edit-code-in-the-app-service-editor"></a><a name="online-editor"></a>Vorgehensweise: Bearbeiten von Code im App Service-Editor
Im Azure-Portal können Sie Ihre Node.js-Back-End-Skriptdateien im App Service-Editor bearbeiten, ohne dass Sie das Projekt auf den lokalen Computer herunterladen müssen. So bearbeiten Sie Skriptdateien im Online-Editor

1. Klicken Sie auf dem Blatt für Ihr Mobile App-Back-End auf **Alle Einstellungen** und dann entweder auf **Einfache Tabellen** oder **Einfache APIs**, eine Tabelle oder API und dann auf **Skript bearbeiten**. Die Skriptdatei wird im App Service-Editor geöffnet.
   
    ![App Service-Editor](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. Nehmen Sie im Online-Editor die gewünschten Änderungen an der Codedatei vor. Die Änderungen werden beim Eingeben automatisch gespeichert.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android-Client-Schnellstart]: app-service-mobile-android-get-started.md
[Apache Cordova-Client-Schnellstart]: app-service-mobile-cordova-get-started.md
[iOS-Client-Schnellstart]: app-service-mobile-ios-get-started.md
[Xamarin.iOS-Client-Schnellstart]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android-Client-Schnellstart]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms-Client-Schnellstart]: app-service-mobile-xamarin-forms-get-started.md
[Windows Store-Client-Schnellstart]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/JavaScript-Client-Schnellstart]: app-service-html-get-started.md
[Synchronisierung von Offlinedaten]: app-service-mobile-offline-data-sync.md
[So wird‘s gemacht: Konfigurieren der Azure Active Directory-Authentifizierung]: app-service-mobile-how-to-configure-active-directory-authentication.md
[So wird‘s gemacht: Konfigurieren der Facebook-Authentifizierung]: app-service-mobile-how-to-configure-facebook-authentication.md
[So wird‘s gemacht: Konfigurieren der Google-Authentifizierung]: app-service-mobile-how-to-configure-google-authentication.md
[So wird‘s gemacht: Konfigurieren der Microsoft-Authentifizierung]: app-service-mobile-how-to-configure-microsoft-authentication.md
[So wird‘s gemacht: Konfigurieren der Twitter-Authentifizierung]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App Service-Bereitstellungshandbuch]: ../app-service-web/web-sites-deploy.md
[Überwachen eines Azure App Service]: ../app-service-web/web-sites-monitor.md
[Aktivieren der Diagnoseprotokollierung in Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Problembehandlung für einen Azure App Service in Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[Angeben der Node.js-Version]: ../nodejs-specify-node-version-azure-apps.md
[Verwenden von Node.js-Modulen]: ../nodejs-use-node-modules-azure-apps.md
[Erstellen eines neuen Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure-Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Zusage]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp-Beispiel auf GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo-Beispiel auf GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[Verzeichnis mit den Beispielen auf GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema-Beispiel auf GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js-Tools 1.1 für Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql-Node.js-Paket]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston



<!--HONumber=Nov16_HO3-->


