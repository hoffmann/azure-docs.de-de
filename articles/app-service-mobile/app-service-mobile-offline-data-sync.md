---
title: Synchronisierung von Offlinedaten in Azure Mobile Apps | Microsoft-Dokumentation
description: "Eine grundlegende Übersicht über das Feature zur Synchronisierung von Offlinedaten für Azure Mobile Apps"
documentationcenter: windows
author: adrianhall
manager: dwrede
editor: 
services: app-service\mobile
ms.assetid: 982fb683-8884-40da-96e6-77eeca2500e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 815de09d9d512caaaef6860b1e5c858202457201


---
# <a name="offline-data-sync-in-azure-mobile-apps"></a>Synchronisierung von Offlinedaten in Azure Mobile Apps
## <a name="what-is-offline-data-sync"></a>Was ist die Synchronisierung von Offlinedateng?
Die Synchronisierung von Offlinedaten ist ein Client- und SDK-Feature von Azure Mobile Apps, die Entwicklern das Erstellen von Apps erleichtert, die ohne Netzwerkverbindung funktionsfähig sind.

Wenn Ihre App im Offlinemodus ist, können Benutzer weiterhin Daten erstellen und ändern. Diese Daten werden in einem lokalen Speicher abgelegt. Wenn die App wieder online ist, können sie lokale Änderungen mit dem Azure Mobile App-Back-End synchronisieren. Das Feature unterstützt auch das Erkennen von Konflikten, wenn auf dem Client und dem Back-End der gleiche Datensatz geändert wird. Konflikte können dann entweder auf dem Server oder vom Client verarbeitet werden.

Die Offlinesynchronisierung bietet eine Reihe an Vorteilen:

* Verbesserte Reaktionsfähigkeit der App durch das lokale Caching von Serverdaten auf dem Gerät
* Erstellen stabiler Apps, die auch bei Netzwerkproblemen funktionstüchtig bleiben
* Endanwender können Daten selbst dann erstellen und ändern, wenn sie keinen Netzwerkzugriff haben, also in Szenarien mit wenig oder keiner Konnektivität
* Daten auf mehreren Geräten synchronisieren und Konflikte erkennen, wenn derselbe Datensatz von zwei Geräte aus geändert wird
* Das Beschränken der Netzwerknutzung bei Netzwerken mit hoher Latenz oder getakteten Netzwerken

Die folgenden Lernprogramme zeigen, wie Sie die Offlinesynchronisierung zu Ihren mobilen Clients mithilfe von Azure Mobile Apps hinzufügen können:

* [Android: Offlinesynchronisierung aktivieren]
* [iOS: Offlinesynchronisierung aktivieren]
* [Xamarin iOS: Offlinesynchronisierung aktivieren]
* [Xamarin Android: Offlinesynchronisierung aktivieren]
* [Xamarin.Forms: Offlinesynchronisierung aktivieren](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Aktivieren der Offlinesynchronisierung für Ihre Windows-App]

## <a name="what-is-a-sync-table"></a>Was ist eine Synchronisierungstabelle?
Um auf den "/tables"-Endpunkt zuzugreifen, stellen Azure Mobile-Client-SDKs Schnittstellen bereit, z. B. `IMobileServiceTable` (.NET-Client-SDK) oder `MSTable` (iOS-Client). Diese APIs stellen eine direkte Verbindung zum Azure Mobile App-Back-End her und schlagen fehl, wenn das Clientgerät keine Netzwerkverbindung hat.

Zur Unterstützung der Offlinenutzung sollte Ihre App stattdessen die *Synchronisierungstabellen*-APIs verwenden, z. B. `IMobileServiceSyncTable` (.NET-Client-SDK) oder `MSSyncTable` (iOS-Client). Die gleichen CRUD-Vorgänge (Erstellen, Lesen, Aktualisieren, Löschen) funktionieren für Synchronisierungstabellen-APIs, mit dem Unterschied, dass sie jetzt einen *lokalen Speicher* lesen oder in diesen schreiben. Bevor Synchronisierungtabellenvorgänge durchgeführt werden können, muss der lokale Speicher initialisiert werden.

## <a name="what-is-a-local-store"></a>Was ist ein lokaler Speicher?
Ein lokaler Speicher ist die Datenpersistenzebene auf dem Clientgerät. Die Client-SDKs von Azure Mobile Apps bieten eine Standardimplementierung für den lokalen Speicher. Unter Windows, Xamarin und Android basieren sie auf SQLite, unter iOS auf Core Data.

Um die SQLite-basierte Implementierung für Windows Phone oder Windows Store 8.1 verwenden zu können, müssen Sie eine SQLite-Erweiterung installieren. Weitere Informationen finden Sie unter [Aktivieren der Offlinesynchronisierung für Ihre Windows-App]. Im Lieferumfang von Android und iOS ist im Betriebssystem selbst eine Version von SQLite enthalten, weshalb es nicht notwendig ist, auf eine eigene Version von SQLite zu verweisen.

Entwickler können auch ihren eigenen lokalen Speicher implementieren. Wenn Sie Daten in einem verschlüsselten Format auf dem mobilen Client speichern möchten, können Sie z. B. einen lokalen Speicher definieren, der SQLCipher für die Verschlüsselung verwendet.

## <a name="what-is-a-sync-context"></a>Was ist ein Synchronisierungskontext?
Ein *Synchronisierungskontext* ist einem mobilen Clientobjekt zugeordnet (z. B. `IMobileServiceClient` oder `MSClient`) und verfolgt die Änderungen, die mithilfe von Synchronisierungstabellen vorgenommen werden. Der Synchronisierungskontext verwaltet eine *Vorgangswarteschlange* mit einer geordneten Liste an CUD-Vorgängen (Erstellen, Aktualisieren und Löschen), die später an den Server gesendet wird.

Ein lokaler Speicher wird mithilfe einer Initialisierungsmethode wie `IMobileServicesSyncContext.InitializeAsync(localstore)` im [.NET Client SDK]dem Synchronisierungskontext zugeordnet.

## <a name="a-namehow-sync-worksahow-offline-synchronization-works"></a><a name="how-sync-works"></a>So funktioniert die Offlinesynchronisierung
Beim Synchronisieren von Tabellen steuert der Clientcode, wann lokale Änderungen mit einem Azure Mobile App-Back-End synchronisiert werden sollen. Nichts wird an das Back-End gesendet, bis ein Aufruf für einen *Pushvorgang* für lokale Änderungen erfolgt. Auf ähnliche Weise wird der lokale Speicher nur dann mit neuen Daten aufgefüllt, wenn ein Aufruf für einen *Pullvorgang* von Daten erfolgt.

* **Pushvorgang**: Push ist ein Vorgang auf dem Synchronisierungskontext, der alle CUD-Änderungen seit dem letzten Pushvorgang sendet. Beachten Sie, dass es nicht möglich ist, nur die Änderungen einer einzelnen Tabelle zu senden, da andernfalls Vorgänge außerhalb der Reihenfolge gesendet werden könnten. Ein Pushvorgang führt eine Reihe von REST-Aufrufen an Ihr Azure Mobile App-Back-End durch, das wiederum die Serverdatenbank ändert.
* **Pullvorgänge**: Pullvorgänge erfolgen pro Tabelle und können mit einer Abfrage so angepasst werden, dass nur eine Teilmenge der Serverdaten abgerufen wird. Die Azure Mobile-Client-SDKs legen dann die resultierenden Daten im lokalen Speicher ab.
* **Implizite Pushvorgänge**: Wenn ein Pullvorgang gegen eine Tabelle mit ausstehenden lokalen Updates ausgeführt wird, führt der Pullvorgang zunächst einen Pushvorgang im Synchronisierungskontext aus. Auf diese Weise minimieren Sie Konflikte zwischen Änderungen, die sich bereits in der Warteschlange befinden, und den neuen Daten vom Server.
* **Inkrementelle Synchronisierung**: der erste Parameter für den Pullvorgang ist ein *Abfragename* , der nur auf dem Client verwendet wird. Wenn Sie eine nicht-Null-Abfrage verwenden, führt das Azure Mobile-SDK eine *inkrementelle Synchronisierung*durch.
  Jedes Mal, wenn ein Pullvorgang einen Satz von Ergebnissen zurückgibt, wird der neueste `updatedAt`-Zeitstempel aus der Ergebnismenge in den lokalen SDK-Systemtabellen gespeichert. Nachfolgende Pullvorgänge rufen nur Datensätze nach dem jeweiligen Zeitstempel ab.
  
  Um die inkrementelle Synchronisierung verwenden zu können, muss Ihr Server sinnvolle `updatedAt`-Werte zurückgeben und das Sortieren nach diesem Feld unterstützen. Da das SDK jedoch eine eigene Sortierung auf das Feld "UpdatedAt" hinzufügt, können Sie keine Pullabfrage nutzen, die über eine eigene `$orderBy$` -Klausel verfügt.
  
  Der Name der Abfrage kann eine beliebige Zeichenfolge sein, muss aber für jede logische Abfrage in Ihrer App eindeutig sein.
  Andernfalls könnten unterschiedliche Pullvorgänge den gleichen Zeitstempel für die inkrementelle Synchronisierung überschreiben, sodass Ihre Abfragen falsche Ergebnisse zurückgeben.
  
  Wenn die Abfrage einen Parameter aufweist, ist das Integrieren des Parameterwerts eine Möglichkeit zum Erstellen eindeutiger Abfragenamen.
  Wenn Sie beispielsweise nach der Benutzer-ID filtern, könnte Ihr eindeutiger Abfragename so aussehen (in C#):
  
        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));
  
  Wenn Sie die inkrementelle Synchronisierung deaktivieren möchten, übergeben Sie `null` als Abfrage-ID. In diesem Fall werden alle Datensätze bei jedem Aufruf von `PullAsync`abgerufen, was möglicherweise ineffizient ist.
* **Löschen**: Sie können den Inhalt des lokalen Speichers mit `IMobileServiceSyncTable.PurgeAsync` löschen.
  Dies kann erforderlich sein, wenn Sie veraltete Daten in der Clientdatenbank haben, oder wenn Sie alle ausstehende Änderungen verwerfen möchten.
  
  Eine Aufräumaktion löscht eine Tabelle aus dem lokalen Speicher. Wenn es Vorgänge der ausstehenden Synchronisierung mit der Datenbank gibt, löst das Löschen eine Ausnahme aus, sofern nicht der Parameter *Löschen erzwingen* festgelegt ist.
  
  Als Beispiel für veraltete Daten auf dem Client nehmen wir an, dass im Beispiel "todo list" Gerät 1 nur Elemente abruft, die nicht abgeschlossen sind. Das todo-Element "Milch kaufen" wird dann auf dem Server von einem anderen Gerät als abgeschlossen markiert. Allerdings verfügt Gerät 1 weiterhin über das todo-Element "Milch kaufen" im lokalen Speicher, da es nur Elemente abruft, die nicht als abgeschlossen markiert sind. Eine Aufräumaktion löscht dieses veraltete Element.

## <a name="next-steps"></a>Nächste Schritte
* [iOS: Offlinesynchronisierung aktivieren]
* [Xamarin iOS: Offlinesynchronisierung aktivieren]
* [Xamarin Android: Offlinesynchronisierung aktivieren]
* [Aktivieren der Offlinesynchronisierung für Ihre Windows-App]

<!-- Links -->
[.NET Client SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Offlinesynchronisierung aktivieren]: app-service-mobile-android-get-started-offline-data.md
[iOS: Offlinesynchronisierung aktivieren]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: Offlinesynchronisierung aktivieren]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Offlinesynchronisierung aktivieren]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Aktivieren der Offlinesynchronisierung für Ihre Windows-App]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md



<!--HONumber=Nov16_HO3-->


