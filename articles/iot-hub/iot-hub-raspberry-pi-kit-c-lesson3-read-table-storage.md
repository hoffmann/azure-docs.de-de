---
title: Lesen von Nachrichten in Azure Storage | Microsoft-Dokumentation
description: "Sie überwachen D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud), während diese in Azure Table Storage geschrieben werden."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Daten aus der Cloud abrufen, IoT-Clouddienst
ms.assetid: 8c5558bb-3c31-4445-90e6-b1a978738545
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 155e5d6280d86b06b1718fc3032c2c224539183d
ms.openlocfilehash: 4e1107400ef08cc3fd955d693c77f332446ecc37


---
# <a name="read-messages-persisted-in-azure-storage"></a>Lesen von Nachrichten in Azure Storage
## <a name="what-you-will-do"></a>Aufgaben
Sie überwachen die D2C-Nachrichten, die von Raspberry Pi 3 an IoT Hub gesendet werden, während diese in Azure Table Storage geschrieben werden. Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel erfahren Sie, wie Sie den Gulp-Task „read-message“ zum Lesen von Nachrichten verwenden, die in Azure Table Storage gespeichert sind.

## <a name="what-you-need"></a>Erforderliches Element
Bevor Sie diesen Prozess starten, müssen Sie den Abschnitt [Run a sample application to send device-to-cloud messages](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md) (Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten) erfolgreich abgeschlossen haben.

## <a name="read-new-messages-from-your-storage-account"></a>Lesen neuer Nachrichten aus dem Speicherkonto
Im vorherigen Artikel haben Sie eine Beispielanwendung auf Pi ausgeführt. Die Beispielanwendung hat Nachrichten an Ihre Azure IoT Hub-Instanz gesendet. Die an Ihren IoT-Hub gesendeten Nachrichten wurden über die Azure-Funktionen-App in Azure Table Storage gespeichert. Sie benötigen die Azure Storage-Verbindungszeichenfolge für das Lesen von Nachrichten aus Azure Table Storage.

Gehen Sie folgendermaßen vor, um Nachrichten zu lesen, die in Azure Table Storage gespeichert sind:

1. Fragen Sie die Verbindungszeichenfolge ab, indem Sie die folgenden Befehle ausführen:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   Der erste Befehl ruft den `storage name` ab, der im zweiten Befehl zum Abrufen der Verbindungszeichenfolge verwendet wird. Verwenden Sie `iot-sample` als Wert von `{resource group name}`, sofern Sie ihn nicht geändert haben.
2. Öffnen Sie die Konfigurationsdatei `config-raspberrypi.json` in Visual Studio Code, indem Sie den folgenden Befehl ausführen:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
3. Ersetzen Sie `[Azure storage connection string]` durch die Verbindungszeichenfolge, die Sie in Schritt 1 abgerufen haben.
4. Speichern Sie die Datei `config-raspberrypi.json`.
5. Senden Sie Nachrichten erneut, und lesen Sie diese aus Azure Table Storage, indem Sie den folgenden Befehl ausführen:
   
   ```bash
   gulp run --read-storage
   ```
   
   Die Logik für das Lesen in Azure Table Storage ist in der Datei `azure-table.js` enthalten.
   
   ![gulp run --read-storage](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message_c.png)

## <a name="summary"></a>Zusammenfassung
Sie haben Pi erfolgreich mit Ihrer IoT Hub-Instanz in der Cloud verbunden und die Beispielanwendung blink verwendet, um D2C-Nachrichten zu senden. Sie können die Azure-Funktionen-App auch dazu verwenden, eingehende IoT Hub-Nachrichten in Azure Table Storage zu speichern. Sie können nun C2D-Nachrichten von IoT Hub an Pi senden.

## <a name="next-steps"></a>Nächste Schritte
[Ausführen einer Beispielanwendung zum Empfangen von C2D-Nachrichten](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md)




<!--HONumber=Dec16_HO1-->


