---
title: "Visualisieren und Problembehandlung von Stream Analytics-Aufträgen | Microsoft Docs"
description: Erfahren Sie, wie Sie eine Stream Analytics-Auftragspipeline visualisieren, um Probleme mithilfe der Diagnosediagrammfunktion selbst zu behandeln.
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: d87841cd-c59f-4a46-b46e-8b904fdc12e9
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/24/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 0aa754ee248b98395bfc9c7fd53187ef702a6d1e


---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Visualisieren und Problembehandlung von Stream Analytics-Aufträgen
Ähnlich wie bei anderen cloudbasierten Technologien ist bei Stream Analytics manchmal eine Problembehandlung notwendig, um zu untersuchen, warum ein Auftrag nicht das erwartete Ergebnis (oder gar kein Ergebnis) liefert. Zu diesem Zweck bietet Stream Analytics die Möglichkeit, einen Streamingauftrag zu visualisieren. Diese Option lässt sich auch als Modellierungstool einsetzen und bietet zudem Vorteile für Benutzer, die eine Dokumentation ihrer Arbeit benötigen.

Im Visualisierungsbereich werden sowohl die Eingaben als auch die ausgeführte Abfrage sowie alle konfigurierten Ausgaben angezeigt. Verbindungs- oder Konfigurationsproblem werden so offensichtlicher, und eine visuelle Darstellung Ihrer Konfiguration kann ebenfalls nützlich sein.

## <a name="using-the-diagnosis-diagram-tool"></a>Verwenden des Diagrammtools für die Diagnose
Um auf diese Schnellansicht zuzugreifen, klicken Sie auf dem Blatt „Einstellungen“ des Stream Analytics-Auftrags auf die Schaltfläche „Diagnosediagramm“.

![stream-analytics-troubleshoot-visualization-diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Die Ein- und Ausgaben sind farbcodiert, um den aktuellen Status der betreffenden Komponente anzugeben, wie unten gezeigt.

![stream-analytics-troubleshoot-visualization-input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Wenn ein Benutzer Zwischenschritte von Abfragen anzeigen möchte, um die Datenflussmuster in einem Auftrag zu verstehen, bietet das Visualisierungstool eine Strukturansicht der Abfrage, einschließlich der Komponentenschritte und der Datenflusssequenz. Durch Klicken auf die einzelnen Schritte der Abfrage wird ein entsprechender Abschnitt in einem Bereich zur Bearbeitung der Abfrage angezeigt, wie unten dargestellt. 

![stream-analytics-troubleshoot-visualization-intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)




<!--HONumber=Nov16_HO3-->


