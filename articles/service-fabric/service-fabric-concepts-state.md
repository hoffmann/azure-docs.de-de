---
title: "Definieren und Verwalten von Zuständen | Microsoft Docs"
description: Definieren und Verwalten des Dienststatus in Service Fabric
services: service-fabric
documentationcenter: .net
author: appi101
manager: timlt
editor: 
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/10/2016
ms.author: aprameyr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: bb27c2f2224791a6a30819da5ee0f09ad7bfa95c


---
# <a name="service-state"></a>Dienstzustand
**Dienstzustand** bezieht sich auf die Daten, die der Dienst für seine Ausführung benötigt. Die Ausführung des Diensts besteht im Lesen und Schreiben der Datenstrukturen und Variablen.

Stellen Sie sich beispielsweise einen einfachen Rechnerdienst vor. Der Dienst berechnet die Summe von zwei Zahlen und gibt die Summe zurück. Es handelt sich hierbei eindeutig um einen zustandslosen Dienst, da mit dem Dienst keinerlei Daten assoziiert sind.

Angenommen, der gleiche Rechner verfügt zusätzlich zur Methode zum Berechnen der Summe über eine Methode zum Zurückgeben der zuletzt berechneten Summe. Dieser Dienst ist damit zustandsbehaftet, da er über einen Zustand verfügt, den er schreibt (beim Berechnen einer neuen Summe) und aus dem er liest (beim Zurückgeben der zuletzt berechneten Summe).

In Azure Service Fabric wird der erste Dienst als „zustandsloser Dienst“ bezeichnet. Der zweite Dienst wird „zustandsbehafteter Dienst“ genannt.

## <a name="storing-service-state"></a>Speichern des Dienstzustands
Der Zustand kann ausgelagert oder mit dem Code, der den Zustand bearbeitet, gespeichert werden. Die Auslagerung des Status erfolgt in der Regel mit einer externen Datenbank oder einem externen Speicher. In unserem Rechnerbeispiel kann beispielsweise eine SQL-Datenbank verwendet werden, in der das aktuelle Ergebnis in einer Tabelle gespeichert ist. Bei jeder Anforderung zum Berechnen der Summe wird diese Zeile aktualisiert.

Der Status kann auch gemeinsam mit dem Code gespeichert werden, die diesen Status bearbeitet. Auf diesem Modell basieren die statusbehafteten Dienste in Service Fabric. Eine entsprechende Infrastruktur in Service Fabric sorgt dafür, dass der Zustand bei einem Ausfall hochverfügbar und fehlertolerant ist.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Service Fabric-Konzepten finden Sie hier:

* [Verfügbarkeit der Service Fabric-Dienste](service-fabric-availability-services.md)
* [Scaling Service Fabric Applications (in englischer Sprache)](service-fabric-concepts-scalability.md)
* [Partitionieren von Service Fabric-Diensten](service-fabric-concepts-partitioning.md)




<!--HONumber=Nov16_HO3-->


