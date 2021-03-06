---
title: Untersuchen von Metriken in Application Insights | Microsoft Docs
description: "Interpretieren von Metrik-Explorer-Diagrammen und Anpassen von Metrik-Explorer-Blättern"
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: dea21a59b189d1d3d474cbc5e67f64df485a1981
ms.openlocfilehash: 040556d42f246ab2876cc5bab031036ec78e13bc


---
# <a name="exploring-metrics-in-application-insights"></a>Untersuchen von Metriken in Application Insights
Metriken in [Application Insights][start] sind gemessene Werte und Anzahlen von Ereignissen, die als Telemetriedaten aus Ihrer Anwendung gesendet werden. Sie dienen zum Erkennen von Leistungsproblemen und Überwachen von Trends bei der Nutzung Ihrer Anwendung. Es gibt eine Vielzahl von Standardmetriken, doch Sie können auch eigene benutzerdefinierte Metriken und Ereignisse erstellen.

Metriken und Ereignisanzahlen werden in Diagrammen aggregierter Werte z. B. als Summen, Mittelwerte oder Zählwerte angezeigt.

Hier sehen Sie ein Beispieldiagramm:

![Öffnen Sie das Blatt "Übersicht" Ihrer Anwendung im Azure-Portal](./media/app-insights-metrics-explorer/01-overview.png)

Einige Diagramme sind unterteilt: die Gesamthöhe des Diagramms an einem beliebigen Punkt ist die Summe der gezeigten Metriken. In der Legende werden in der Standardeinstellung die größten Mengen gezeigt.

Gepunktete Linien zeigen den Wert der Metrik eine Woche zuvor.

## <a name="time-range"></a>Zeitbereich
Sie können den Zeitbereich der Diagramme oder Raster auf jedem Blatt ändern.

![Öffnen Sie das Blatt "Übersicht" Ihrer Anwendung im Azure-Portal](./media/app-insights-metrics-explorer/03-range.png)

Klicken Sie auf "Aktualisieren", wenn Sie einige Daten erwarten, die noch nicht angezeigt wurden. Diagramme aktualisieren sich regelmäßig selbst, wobei die Intervalle für größere Zeiträume länger sind. Im Freigabemodus kann es eine Weile dauern, bis Daten durch die Analysepipeline in ein Diagramm gelangen.

Zum Vergrößern eines Teils eines Diagramms ziehen Sie die Maus darüber:

![Ziehen Sie die Maus über einen Teil eines Diagramms.](./media/app-insights-metrics-explorer/12-drag.png)

Klicken Sie auf die Schaltfläche zum Aufheben der Vergrößerung, um es wiederherzustellen.

## <a name="granularity-and-point-values"></a>Granularität und Punktwerte
Bewegen Sie die Maus über dem Diagramm, um die Werte der Metriken zu diesem Zeitpunkt anzuzeigen.

![Bewegen Sie den Mauszeiger über einem Diagramm](./media/app-insights-metrics-explorer/02-focus.png)

Der Wert der Eigenschaft zu einem bestimmten Zeitpunkt wird über das vorherige Abtastintervall aggregiert.

Das Abtastintervall, auch als „Granularität“ bezeichnet, wird oben auf dem Blatt angezeigt.

![Die Kopfzeile eines Blatts.](./media/app-insights-metrics-explorer/11-grain.png)

Sie können die Granularität auf dem Blatt „Zeitbereich“ anpassen:

![Die Kopfzeile eines Blatts.](./media/app-insights-metrics-explorer/grain.png)

Die verfügbaren Granularitäten hängen vom ausgewählten Zeitbereich ab. Die expliziten Granularitäten sind Alternativen für die „automatische“ Granularität für den Zeitbereich.

## <a name="metrics-explorer"></a>Metrik-Explorer
Klicken Sie sich auf dem Blatt "Übersicht" durch ein beliebiges Diagramm, um eine detailliertere Gruppe zusammengehöriger Diagramme und Raster anzuzeigen. Sie können diese Diagramme und Raster bearbeiten, um sich auf die Details zu konzentrieren, die Sie interessieren.

Sie können auch einfach im Metrik-Explorer auf die Kopfzeile des Übersichtsblatts klicken.

Klicken Sie sich z. B. durch das Diagramm "Anforderungsfehler" der Web-App:

![Klicken Sie auf dem Blatt "Übersicht" auf ein Diagramm](./media/app-insights-metrics-explorer/14-trix.png)

## <a name="what-do-the-figures-mean"></a>Was bedeuten die Zahlen?
In der Legende auf der Seite wird standardmäßig der über den Zeitraum des Diagramms aggregierte Wert angezeigt. Wenn Sie auf das Diagramm zeigen, wird der an diesem Punkt befindliche Wert angezeigt.

Jeder Datenpunkt im Diagramm ist ein Aggregat der Datenwerte, die im vorherigen Samplingintervall (Granularität) empfangen wurden. Die Granularität wird oben auf dem Blatt angezeigt und variiert abhängig von der gesamten Zeitskala des Diagramms.

Metriken können auf unterschiedliche Weisen zusammengefasst werden:

* **Summe** addiert die Werte aller Datenpunkte, die über das Samplingintervall oder den Zeitraum des Diagramms empfangen werden.
* **Durchschnitt** teilt die Summe durch die Anzahl der Datenpunkte, die während des Intervalls empfangen werden.
* **Eindeutige** Werte werden verwendet, um Benutzer und Konten zu zählen. Die Abbildung zeigt die Anzahl der verschiedenen Benutzer, die während des Samplingintervalls oder über den Zeitraum des Diagramms gezählt werden.

Sie können die Aggregationsmethode ändern:

![Wählen Sie das Diagramm und dann die Aggregation aus](./media/app-insights-metrics-explorer/05-aggregation.png)

Die Standardmethode für die einzelnen Metriken wird bei der Erstellung eines neuen Diagramms bzw. bei Deaktivierung der gesamten Metrikauswahl angezeigt:

![Heben Sie die Auswahl aller Metriken auf, um die Standardwerte anzuzeigen](./media/app-insights-metrics-explorer/06-total.png)

## <a name="editing-charts-and-grids"></a>Bearbeiten von Diagrammen und Rastern
So fügen Sie dem Blatt ein neues Diagramm hinzu:

![Wählen Sie im Metrik-Explorer "Diagramm hinzufügen"](./media/app-insights-metrics-explorer/04-add.png)

Wählen Sie in einem vorhandenen oder neuen Diagramm die Option **Bearbeiten** , um dessen Anzeige zu bearbeiten:

![Wählen Sie eine oder mehrere Metriken aus](./media/app-insights-metrics-explorer/08-select.png)

Sie können in einem Diagramm mehr als eine Metrik anzeigen, wenngleich es Einschränkungen bezüglich der Kombinationen gibt, die zusammen angezeigt werden können. Sobald Sie eine Metrik auswählen, werden einige der anderen deaktiviert.

Wenn Sie [benutzerdefinierte Metriken][track] in Ihrer App (Aufrufe von „TrackMetric“ und „TrackEvent“) programmiert haben, werden sie hier aufgelistet.

## <a name="segment-your-data"></a>Segmentieren der Daten
Sie können eine Metrik nach Eigenschaften aufteilen, um beispielsweise Seitenansichten auf Clients mit unterschiedlichen Betriebssystemen zu vergleichen.

Wählen Sie ein Diagramm oder Raster aus, aktivieren Sie das Gruppieren, und wählen Sie eine Eigenschaft, nach der gruppiert werden soll:

![Wählen Sie "Gruppierung EIN" und dann in "Gruppieren nach" eine Eigenschaft aus](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> Wenn Sie die Gruppierung verwenden, zeigen die Diagrammtypen „Fläche“ und „Balken“ eine gestapelte Ansicht. Dies ist nützlich, wenn die Aggregationsmethode „Sum“ lautet. Wird als Aggregationstyp aber „Durchschnitt“ verwendet, wählen Sie die Anzeigetypen „Zeile“ oder „Raster“.
>
>

Wenn Sie [benutzerdefinierte Metriken][track] in Ihrer App programmiert haben und diese Eigenschaftswerte enthalten, können Sie die Eigenschaft in der Liste auswählen.

Ist das Diagramm für segmentierte Daten zu klein? Passen Sie die Höhe an:

![Passen Sie den Schieberegler an](./media/app-insights-metrics-explorer/18-height.png)

## <a name="filter-your-data"></a>Filtern der Daten
So zeigen Sie nur die Metriken für eine ausgewählte Gruppe von Eigenschaftswerten an:

![Klicken Sie auf "Filter", erweitern Sie eine Eigenschaft, und überprüfen Sie einige Werte](./media/app-insights-metrics-explorer/19-filter.png)

Wenn Sie für eine bestimmte Eigenschaft keine Werte auswählen, entspricht dies der Auswahl aller Werte ohne Angabe von Filtern.

Beachten Sie die Anzahlen von Ereignissen neben jedem Eigenschaftswert. Wenn Sie Werte einer Eigenschaft auswählen, werden neben anderen Eigenschaftswerten die Anzahlen angepasst.

Filter gelten für alle Diagramme eines Blatts. Falls unterschiedliche Filter auf unterschiedliche Diagramme angewendet werden sollen, müssen Sie unterschiedliche Blätter mit Metriken erstellen und speichern. Sie können bei Bedarf auch Diagramme von unterschiedlichen Blättern auf dem Dashboard anheften, damit sie nebeneinander angezeigt werden.

### <a name="remove-bot-and-web-test-traffic"></a>Entfernen von Robot- und Webtest-Datenverkehr
Verwenden Sie den Filter **Realer oder synthetischer Datenverkehr**, und aktivieren Sie **Real**.

Sie können auch nach **Quelle von synthetischem Datenverkehr**filtern.

### <a name="to-add-properties-to-the-filter-list"></a>So fügen Sie der Filterliste Eigenschaften hinzu
Möchten Sie Telemetrie nach einer Kategorie Ihrer Wahl filtern? Möglicherweise unterteilen Sie Ihre Benutzer in verschiedene Kategorien und möchten Ihre Daten nach diesen Kategorien segmentieren.

[Erstellen Sie eine eigene Eigenschaft](app-insights-api-custom-events-metrics.md#properties). Legen Sie sie in einem [Telemetrieinitialisierer](app-insights-api-custom-events-metrics.md#defaults) fest, damit sie in allen Telemetrieformen angezeigt wird, einschließlich der von unterschiedlichen SDK-Modulen gesendeten Standardtelemetrie.

## <a name="edit-the-chart-type"></a>Bearbeiten des Diagrammtyps
Beachten Sie, dass Sie zwischen Rastern und Diagrammen wechseln können:

![Wählen Sie ein Raster oder Diagramm und anschließend einen Diagrammtyp](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>Speichern des Blatts "Metriken"
Wenn Sie einige Diagramme erstellt haben, speichern Sie sie als Favoriten. Wenn Sie mit einem Organisationskonto arbeiten, können Sie wählen, ob Sie sie für andere Teammitglieder freigeben.

![Wählen Sie "Favoriten"](./media/app-insights-metrics-explorer/21-favorite-save.png)

Um das Blatt erneut zu verwenden, **wechseln Sie zum Blatt "Übersicht"** und öffnen "Favoriten":

![Wählen Sie "Favoriten" auf dem Blatt "Übersicht" aus](./media/app-insights-metrics-explorer/22-favorite-get.png)

Wenn Sie beim Speichern den Zeitbereich "Relativ" ausgewählt haben, wird das Fenster mit den neuesten Metriken aktualisiert. Wenn Sie den Zeitraum "Absolut" gewählt haben, werden jedes Mal dieselben Daten gezeigt.

## <a name="reset-the-blade"></a>Zurücksetzen des Blatts
Wenn Sie ein Blatt bearbeiten, aber dann lieber wieder mit den ursprünglich gespeicherten Daten weiterarbeiten möchten, klicken Sie auf "Zurücksetzen".

![In den Schaltflächen am oberen Rand des Metrik-Explorers](./media/app-insights-metrics-explorer/17-reset.png)

<a name="live-metrics-stream"></a>

## <a name="live-metrics-stream-instant-metrics-for-close-monitoring"></a>Live Metrics Stream: sofortige Metriken für eine genaue Überwachung
Live Metrics Stream zeigt Ihnen Ihre Anwendungsmetriken für den aktuellen Zeitpunkt an – nahezu in Echtzeit, mit einer Latenz von 1 Sekunde. Dies ist äußerst nützlich, wenn Sie einen neuen Build freigeben und sicherstellen möchten, dass alles wie erwartet funktioniert, oder wenn Sie einen Incident in Echtzeit untersuchen möchten.

![Klicken Sie auf dem Blatt „Übersicht“ auf „Livedatenstrom“.](./media/app-insights-metrics-explorer/live-stream.png)

Im Gegensatz zum Metrik-Explorer zeigt Live Metrics Stream einen festen Satz von Metriken an. Die Daten werden nur so lange beibehalten, wie sie im Diagramm angezeigt werden. Anschließend werden sie verworfen.

### <a name="live-failures"></a>Live-Fehler

Wenn Fehler oder Ausnahmen protokolliert werden, wählt Live Stream eine Auswahl daraus aus. Klicken Sie auf **Anhalten**, um ein bestimmtes Beispiel anzuzeigen, und wählen Sie ein Ereignis, um dessen Details anzuzeigen.

![Auswahl der Live-Fehler](./media/app-insights-metrics-explorer/live-stream-failures.png)


Live Metrics Stream steht mit der neuesten Webversion von [Application Insights SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) zur Verfügung.

## <a name="set-alerts"></a>Festlegen von Benachrichtigungen
Fügen Sie eine Benachrichtigung hinzu, wenn Sie per E-Mail über ungewöhnliche Werte einer beliebigen Metrik informiert werden möchten. Sie können auswählen, ob die E-Mail an die Kontoadministratoren oder an bestimmte E-Mail-Adressen gesendet wird.

![Wählen Sie im Metrik-Explorer "Warnungsregeln", "Warnung hinzufügen"](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

[Weitere Informationen zu Warnungen][alerts]

## <a name="export-to-excel"></a>Exportieren in Excel
Sie können im Metrik-Explorer angezeigte Metrikdaten in eine Excel-Datei exportieren. Die exportierten Daten enthalten Daten aus allen Diagrammen und Tabellen, die im Portal angezeigt werden.

![Wählen Sie im Metrik-Explorer "Warnungsregeln", "Warnung hinzufügen"](./media/app-insights-metrics-explorer/31-export.png)

Die Daten jedes Diagramms und jeder Tabelle werden in separate Blätter der Excel-Datei exportiert.

Die Dateien, die Ihnen angezeigt werden, werden exportiert. Ändern Sie den Zeitraum oder die Filter, wenn Sie den zu exportierenden Datenbereich ändern möchten. Wenn bei Tabellen der Befehl **Weitere laden** angezeigt wird, können Sie darauf klicken, bevor Sie auf "Exportieren" klicken, um mehr Daten zu exportieren.

*Der Export funktioniert zurzeit nur mit Internet Explorer und Chrome. Unterstützung für weitere Browser wird in Zukunft verfügbar sein.*

## <a name="continuous-export"></a>Fortlaufender Export
Wenn Sie Daten zur externen Verarbeitung fortlaufend exportieren möchten, sollten Sie die Funktion [Fortlaufender Export](app-insights-export-telemetry.md)verwenden.

### <a name="power-bi"></a>Power BI
Wenn Sie umfangreichere Ansichten der Daten wünschen, können Sie sie [nach Power BI exportieren](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Analyse
[Analytics](app-insights-analytics.md) bietet eine vielseitigere Möglichkeit zum Analysieren Ihrer Telemetriedaten mit einer leistungsfähigen Abfragesprache. Verwenden Sie sie, wenn Sie Ergebnisse von Metriken kombinieren oder berechnen oder eine eingehende Untersuchung der kürzlich erzielten Leistung Ihrer App durchführen möchten. Verwenden Sie dagegen den Metrik-Explorer, wenn Sie eine automatische Aktualisierung, Diagramme im Dashboard und Warnungen verwenden möchten.

## <a name="troubleshooting"></a>Problembehandlung
*In meinem Diagramm werden keine Daten angezeigt.*

* Filter gelten für alle Diagramme im Blatt. Stellen Sie sicher, dass Sie bei der Arbeit mit einem Diagramm keinen Filter festgelegt haben, mit dem alle Daten eines anderen Diagramms ausgeschlossen werden.

    Wenn Sie verschiedene Filter für verschiedene Diagramme festlegen möchten, sollten Sie diese in unterschiedlichen Blättern erstellen und als separate Favoriten speichern. Sie können sie auch im Dashboard anheften, damit sie nebeneinander angezeigt werden.
* Wenn Sie ein Diagramm nach einer Eigenschaft gruppieren, die nicht in der Metrik definiert ist, sind im Diagramm keine Daten enthalten. Versuchen Sie, die Einstellungen für „Gruppieren nach“ zu löschen, oder wählen Sie eine andere Gruppierungseigenschaft aus.
* Leistungsdaten (CPU, E/A-Rate usw.) sind für Java-Webdienste, Windows-Desktop-Apps, [IIS-Web-Apps und -Dienste bei Installation des Statusmonitors](app-insights-monitor-performance-live-website-now.md) und [Azure Cloud Services](app-insights-azure.md) verfügbar. Für Azure-Websites sind sie dagegen nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte
* [Überwachen der Verwendung mit Application Insights](app-insights-overview-usage.md)
* [Verwenden der Diagnosesuche](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md



<!--HONumber=Nov16_HO3-->


