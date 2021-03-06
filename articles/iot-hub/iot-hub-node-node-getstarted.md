---
title: Erste Schritte mit Azure IoT Hub (Node) | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie mithilfe der Azure IoT-SDKs für Node.js D2C-Nachrichten von einem Gerät an einen Azure IoT-Hub senden. Sie erstellen eine App für ein simuliertes Gerät zum Senden von Nachrichten, eine Dienst-App zum Registrieren Ihres Geräts in der Identitätsregistrierung und eine Dienst-App zum Lesen der D2C-Nachrichten aus dem IoT-Hub."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 56618522-9a31-42c6-94bf-55e2233b39ac
ms.service: iot-hub
ms.devlang: javascript
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: a243e4f64b6cd0bf7b0776e938150a352d424ad1
ms.openlocfilehash: 6a4275b7fb7501fec4e98f87b09e20b2114b556b


---
# <a name="get-started-with-azure-iot-hub-node"></a>Erste Schritte mit Azure IoT Hub (Node)
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Tutorials verfügen Sie über drei Node.js-Konsolen-Apps:

* **CreateDeviceIdentity.js**: Hiermit werden eine Geräteidentität und ein zugehöriger Sicherheitsschlüssel zum Verbinden mit Ihrer simulierten Geräte-App erstellt.
* **ReadDeviceToCloudMessages.js**: Hiermit wird die Telemetrie angezeigt, die Ihre simulierte Geräte-App sendet.
* **SimulatedDevice.js**: Hiermit wird mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub hergestellt und jede Sekunde unter Verwendung des AMQP-Protokolls eine Telemetrienachricht gesendet.

> [!NOTE]
> Im Artikel [IoT Hub SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen Azure IoT SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Node.js, Version 0.10.x oder höher.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sie haben nun Ihren IoT Hub erstellt. Sie verfügen über den IoT Hub-Hostnamen und die IoT Hub-Verbindungszeichenfolge, die Sie für die weiteren Schritte in diesem Tutorial benötigen.

## <a name="create-a-device-identity"></a>Erstellen einer Geräteidentität
In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die eine Geräteidentität in der Identitätsregistrierung Ihres IoT Hubs erstellt. Ein Gerät kann nur eine Verbindung mit IoT Hub herstellen, wenn in der Identitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im Abschnitt **Identitätsregistrierung** des [Entwicklungsleitfadens für IoT Hub][lnk-devguide-identity]. Beim Ausführen dieser Konsolen-App werden eine eindeutige Geräte-ID und ein Schlüssel erstellt, mit denen sich das Gerät beim Senden von D2C-Nachrichten (Device-to-Cloud, Gerät-an-Cloud) beim IoT Hub identifizieren kann.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **createdeviceidentity**. Erstellen Sie im Ordner **createdeviceidentity** die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **createdeviceidentity** den folgenden Befehl aus, um das Dienst-SDK-Paket **azure-iothub** zu installieren:
   
    ```
    npm install azure-iothub --save
    ```
3. Erstellen Sie mit einem Text-Editor im Ordner **createdeviceidentity** die Datei **CreateDeviceIdentity.js**.
4. Fügen Sie am Anfang der Datei **CreateDeviceIdentity.js** die folgende `require`-Anweisung hinzu:
   
    ```
    'use strict';
   
    var iothub = require('azure-iothub');
    ```
5. Fügen Sie der Datei **CreateDeviceIdentity.js** den folgenden Code hinzu, und ersetzen Sie den Platzhalterwert durch die IoT Hub-Verbindungszeichenfolge für den Hub, den Sie im vorherigen Abschnitt erstellt haben: 
   
    ```
    var connectionString = '{iothub connection string}';
   
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```
6. Fügen Sie den unten angegebenen Code hinzu, um in der Identitätsregistrierung in Ihrem IoT-Hub eine Gerätedefinition zu erstellen. Mit diesem Code wird ein Gerät erstellt, wenn die Geräte-ID in der Identitätsregistrierung nicht vorhanden ist. Andernfalls wird der Schlüssel des vorhandenen Geräts zurückgegeben:
   
    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });
   
    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device ID: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.symmetricKey.primaryKey);
      }
    }
    ```
7. Speichern und schließen Sie die Datei **CreateDeviceIdentity.js** .
8. Führen Sie zum Ausführen der Anwendung **create-device-identity** den folgenden Befehl an der Eingabeaufforderung im Ordner „createdeviceidentity“ aus:
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. Notieren Sie sich die **Geräte-ID** und den **Geräteschlüssel**. Sie benötigen diese Werte später beim Erstellen einer Anwendung, für die eine Verbindung mit IoT Hub als Gerät hergestellt wird.

> [!NOTE]
> Die Identitätsregistrierung in IoT Hub speichert nur Geräteidentitäten, um einen sicheren Zugriff auf den IoT-Hub zu ermöglichen. Sie speichert Geräte-IDs und Schlüssel, die als Sicherheitsanmeldeinformationen verwendet werden, sowie ein Aktiviert/Deaktiviert-Kennzeichen, mit dem Sie den Zugriff für ein einzelnes Gerät deaktivieren können. Wenn Ihre Anwendung das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity].
> 
> 

## <a name="receive-device-to-cloud-messages"></a>Empfangen von Gerät-an-Cloud-Nachrichten
In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die D2C-Nachrichten (Device-to-Cloud) aus dem IoT Hub liest. Ein IoT-Hub macht einen mit [Event Hubs][lnk-event-hubs-overview] kompatiblen Endpunkt verfügbar, der Ihnen das Lesen von D2C-Nachrichten ermöglicht. Zur Vereinfachung wird in diesem Tutorial ein einfacher Reader erstellt, der für eine Bereitstellung mit hohem Durchsatz nicht geeignet ist. Im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial] wird gezeigt, wie Sie D2C-Nachrichten mit Skalierung verarbeiten. Das Tutorial [Erste Schritte mit Event Hubs][lnk-eventhubs-tutorial] enthält weitere Informationen zum Verarbeiten der Nachrichten von Event Hubs und gilt für Endpunkte, die mit IoT Hub-Event Hubs kompatibel sind.

> [!NOTE]
> Der mit Event Hub kompatible Endpunkt zum Lesen von D2C-Nachrichten verwendet immer das AMQP-Protokoll.
> 
> 

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **readdevicetocloudmessages**. Erstellen Sie im Ordner **readdevicetocloudmessages** die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **readdevicetocloudmessages** den folgenden Befehl aus, um das Paket **azure-event-hubs** zu installieren:
   
    ```
    npm install azure-event-hubs --save
    ```
3. Erstellen Sie mit einem Text-Editor im Ordner **readdevicetocloudmessages** die Datei **ReadDeviceToCloudMessages.js**.
4. Fügen Sie am Anfang der Datei **ReadDeviceToCloudMessages.js** die folgenden `require`-Anweisungen hinzu:
   
    ```
    'use strict';
   
    var EventHubClient = require('azure-event-hubs').Client;
    ```
5. Fügen Sie die folgende Variablendeklaration hinzu, und ersetzen Sie den Platzhalterwert durch die IoT Hub-Verbindungszeichenfolge für Ihren Hub:
   
    ```
    var connectionString = '{iothub connection string}';
    ```
6. Fügen Sie die folgenden beiden Funktionen hinzu, mit denen die Ausgabe an die Konsole erfolgt:
   
    ```
    var printError = function (err) {
      console.log(err.message);
    };
   
    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```
7. Fügen Sie den folgenden Code hinzu, um das **EventHubClient**-Element zu erstellen, öffnen Sie die Verbindung mit Ihrem IoT Hub, und erstellen Sie ein Receiver-Element für jede Partition. Bei dieser Anwendung wird beim Erstellen eines Receiver-Elements ein Filter verwendet, damit das Receiver-Element nur Nachrichten liest, die nach Beginn der Ausführung des Receiver-Elements an IoT Hub gesendet werden. Dies ist in einer Testumgebung hilfreich, damit Sie die Anzeige auf die aktuelle Gruppe von Nachrichten begrenzen können. In einer Produktionsumgebung sollte mit Ihrem Code sichergestellt werden, dass alle Nachrichten verarbeitet werden. Weitere Informationen hierzu finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial]:
   
    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```
8. Speichern und schließen Sie die Datei **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt erstellen Sie eine Node.js-Konsolenanwendung, die ein Gerät simuliert, mit dem D2C-Nachrichten (Device-to-Cloud) an einen IoT Hub gesendet werden.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **simulateddevice**. Erstellen Sie im Ordner **simulateddevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **simulateddevice** den folgenden Befehl aus, um das Dienst-SDK-Paket **azure-iot-device** und das Paket **azure-iot-device-amqp** zu installieren:
   
    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```
3. Erstellen Sie mit einem Text-Editor im Ordner **simulateddevice** die neue Datei **SimulatedDevice.js**.
4. Fügen Sie am Anfang der Datei **SimulatedDevice.js** die folgenden `require`-Anweisungen hinzu:
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. Fügen Sie die Variable **connectionString** hinzu, und verwenden Sie sie zum Erstellen einer **Client**-Instanz. Ersetzen Sie **{youriothostname}** durch den Namen des IoT Hubs, den Sie im Abschnitt *Erstellen eines IoT Hubs* erstellt haben. Ersetzen Sie **{yourdevicekey}** durch den Geräteschlüsselwert, den Sie im Abschnitt *Erstellen einer Geräteidentität* generiert haben:
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```
6. Fügen Sie die folgende Funktion hinzu, um die Ausgabe der Anwendung anzuzeigen:
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Erstellen Sie einen Rückruf, und verwenden Sie die Funktion **setInterval** , um jede Sekunde eine neue Nachricht an Ihren IoT Hub zu senden:
   
    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
   
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
8. Öffnen Sie die Verbindung mit dem IoT Hub, und beginnen Sie mit dem Senden von Nachrichten:
   
    ```
    client.open(connectCallback);
    ```
9. Speichern und schließen Sie die Datei **SimulatedDevice.js** .

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler][lnk-transient-faults] beschrieben.
> 
> 

## <a name="run-the-apps"></a>Ausführen der Apps
Sie können die Apps nun ausführen.

1. Führen Sie an der Eingabeaufforderung im Ordner **readdevicetocloudmessages** den folgenden Befehl aus, um die Überwachung Ihres IoT-Hubs zu starten:
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![Node.js-basierte IoT Hub-Dienst-App zum Überwachen von D2C-Nachrichten][7]
2. Führen Sie an einer Eingabeaufforderung im Ordner **simulateddevice** den folgenden Befehl aus, um mit dem Senden von Telemetriedaten an Ihren IoT-Hub zu beginnen:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![Node.js-basierte IoT Hub-Geräte-App zum Senden von D2C-Nachrichten][8]
3. Über die Kachel **Nutzung** im [Azure-Portal][lnk-portal] wird die Anzahl der an den IoT-Hub gesendeten Nachrichten angezeigt:
   
    ![Azure-Portal-Kachel „Nutzung“ mit der Anzahl von Nachrichten, die an IoT Hub gesendet wurden][43]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie im Azure-Portal einen neuen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um die SimulatedDevice-App für das Senden von D2C-Nachrichten an den IoT-Hub zu aktivieren. Sie haben außerdem eine App erstellt, mit der die vom IoT-Hub empfangenen Nachrichten angezeigt werden. 

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

* [Verbinden Ihres Geräts][lnk-connect-device]
* [Erste Schritte mit der Geräteverwaltung][lnk-device-management]
* [Erste Schritte mit dem IoT-Gateway SDK][lnk-gateway-SDK]

Informationen dazu, wie Sie Ihre IoT-Lösung erweitern und Ihre D2C-Nachrichten mit Skalierbarkeit verarbeiten, finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial].

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Dec16_HO1-->


