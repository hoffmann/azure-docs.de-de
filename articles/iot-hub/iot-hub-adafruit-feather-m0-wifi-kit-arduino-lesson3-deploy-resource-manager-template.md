---
title: Erstellen einer Azure-Funktionen-App und eines Speicherkontos | Microsoft-Dokumentation
description: Die Azure-Funktionen-App lauscht auf Azure IoT Hub-Ereignisse, verarbeitet eingehende Nachrichten und schreibt sie in Azure Table Storage.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Speichern von Daten in der Cloud, in der Cloud gespeicherte Daten, IoT-Clouddienst
ms.assetid: 9c8f4cd1-9511-4601-ad7e-51761a986753
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 946d66cc7ed1173e7b2b92dae90a1ce69ba3c165
ms.openlocfilehash: 18806dc515a9a8951134d580a2c0664ff503e17c


---

# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Erstellen einer Azure-Funktionen-App und eines Azure-Speicherkontos
[Azure Functions](../../articles/azure-functions/functions-overview.md) ist eine Lösung, mit der Sie ganz einfach *Funktionen* (kleinere Codeelemente) in der Cloud ausführen können. Eine Azure-Funktionen-App hostet die Ausführung Ihrer Funktionen in Azure.

## <a name="what-will-you-do"></a>Aufgaben
Verwenden Sie eine Azure Resource Manager-Vorlage zum Erstellen einer Azure-Funktionen-App und eines Azure-Speicherkontos. Die Azure-Funktionen-App lauscht auf Azure IoT Hub-Ereignisse, verarbeitet eingehende Nachrichten und schreibt sie in Azure Table Storage.

Wenn Probleme auftreten, suchen Sie auf der [Problembehandlungsseite](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) nach Lösungen für Ihr Arduino-Board Adafruit Feather M0 WiFi.

## <a name="what-will-you-learn"></a>Lernziele
In diesem Artikel lernen Sie Folgendes:
* Verwenden von [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) zum Bereitstellen von Azure-Ressourcen.
* Verwenden einer Azure-Funktionen-App für das Verarbeiten von IoT Hub-Nachrichten und zum Schreiben der Nachrichten in eine Tabelle in Azure Table Storage.

## <a name="what-do-you-need"></a>Voraussetzungen
Folgende Tutorials müssen erfolgreich abgeschlossen worden sein:
- [Erste Schritte mit Ihrem Arduino-Board][get-started]
- [Erstellen der Azure IoT Hub-Instanz][create-iot-hub]

## <a name="open-the-sample-app"></a>Öffnen der Beispiel-App
Öffnen Sie das Beispielprojekt in Visual Studio Code, indem Sie die folgenden Befehle ausführen:

```bash
cd Lesson3
code .
```

![Repo structure][repo-structure]

* Die Datei `app.ino` im Unterordner `app` ist die Hauptquelldatei. Diese Quelldatei enthält den Code, mit dem eine Nachricht 20-mal an die IoT Hub-Instanz gesendet wird. Dabei blinkt die LED bei jeder gesendeten Nachricht.
* Die Datei `config.json` enthält erforderliche Konfigurationseinstellungen.
* Die Datei `arm-template.json` ist die Azure Resource Manager-Vorlage mit der Azure-Funktionen-App und einem Azure-Speicherkonto.
* Die Datei `arm-template-param.json` ist die Konfigurationsdatei, die von der Azure Resource Manager-Vorlage verwendet wird.
* Der Unterordner `ReceiveDeviceMessages` enthält den Node.js-Code für die Azure-Funktion.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurieren von Azure Resource Manager-Vorlagen und Erstellen von Ressourcen in Azure
Aktualisieren Sie die Datei `arm-template-param.json` in Visual Studio Code.

![Azure Resource Manager-Vorlagenparameter][arm-template-params]

* Ersetzen Sie **[your IoT Hub name]** (Ihr IoT Hub-Name) durch **{mein Hub-Name}**, den Sie beim [Erstellen des IoT Hubs und Registrieren Ihres Arduino-Boards][created-iot-hub-and-registered-arduino-board] angegeben haben.
* Ersetzen Sie **[prefix string for new resources]** durch ein beliebiges Präfix. Durch das Präfix wird sichergestellt, dass der Ressourcenname global eindeutig ist, um Konflikte zu vermeiden. Verwenden Sie beim Präfix keine Gedankenstriche oder Zahlen als erstes Zeichen.

Nach der Aktualisierung der Datei `arm-template-param.json` stellen Sie die Ressourcen in Azure bereit, indem Sie den folgenden Befehl ausführen:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Es dauert etwa fünf Minuten, diese Ressourcen zu erstellen. Sie können während der Ressourcenerstellung mit dem nächsten Artikel fortfahren.

## <a name="summary"></a>Zusammenfassung
Sie haben die Azure-Funktionen-App zum Verarbeiten von IoT Hub-Nachrichten sowie ein Azure-Speicherkonto zum Speichern dieser Nachrichten erstellt. Sie können nun das Beispiel bereitstellen und ausführen, um D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud) an Ihr Arduino-Board zu senden.

## <a name="next-steps"></a>Nächste Schritte
[Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten auf Ihrem Arduino-Board][send-device-to-cloud-messages]

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md


<!--HONumber=Dec16_HO2-->


