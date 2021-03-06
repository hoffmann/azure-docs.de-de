---
title: API-Versionen von Azure Search | Microsoft Docs
description: "Versionsrichtlinie für Azure Search-REST-APIs und die Clientbibliothek im .NET SDK."
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 0458053a-164e-4682-a802-00097ecde981
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: brjohnst
translationtype: Human Translation
ms.sourcegitcommit: fc2f30569acc49dd383ba230271989eca8a14423
ms.openlocfilehash: 66aec5feaa370aef6dc4a9f83119e26ab47d726a

---

# <a name="api-versions-in-azure-search"></a>API-Versionen in Azure Search
Für Azure Search sind regelmäßig Funktionsupdates verfügbar. Manchmal, aber nicht immer, erfordern diese Updates die Veröffentlichung einer neuen Version unserer API, um die Abwärtskompatibilität zu gewährleisten. Die Veröffentlichung einer neuen Version ermöglicht Ihnen, zu steuern, wann und wie Sie Suchdienstupdates in Ihren Code integrieren.

In der Regel versuchen wir, neue Versionen nur im Bedarfsfall zu veröffentlichen, da es einen gewissen Aufwand erfordern kann, wenn Sie Ihren Code aktualisieren, um eine neue API-Version zu verwenden. Wir veröffentlichen nur dann eine neue Version, wenn wir einen Aspekt der API in einer Weise ändern müssen, die die Abwärtskompatibilität beeinträchtigt. Ursache hierfür können Fehlerbehebungen bei vorhandenen Funktionen sein, oder neue Funktionen, die vorhandene API-Oberflächenbereiche ändern.

Wir befolgen die gleiche Regel bei SDK-Updates. Das Azure Search-SDK folgt den Regeln der [semantischen Versionierung](http://semver.org/), d.h., dass seine Version aus drei Teilen besteht: Haupt-, Neben- und Buildnummer (z.B. 1.1.0). Wir veröffentlichen nur dann eine neue Hauptversion des SDK, wenn Änderungen die Abwärtskompatibilität beeinträchtigen. Bei Funktionsupdates ohne Beeinträchtigung der Abwärtskompatibilität setzen wir die Nebenversion herauf, und für Programmfehlerbehebungen setzen wir nur die Buildversion herauf.

## <a name="snapshot-of-current-versions"></a>Momentaufnahme aktueller Versionen
Unten sehen Sie eine Momentaufnahme der aktuellen Versionen aller Programmierschnittstellen zu Azure Search. Roadmaps und weitere Details finden Sie in den nachfolgenden Abschnitten dieses Dokuments.

| Schnittstellen | Letzte Hauptversion | Status |
| --- | --- | --- |
| [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) |1.1 |Allgemein verfügbar, freigegeben Februar 2016 |
| [.NET SDK-Vorschau](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) |2.0-Vorschau |Vorschau, veröffentlicht August 2016 |
| [Dienst-REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) |2016-09-01 |Allgemein verfügbar |
| [Dienst-REST-API (Vorschau)](search-api-2015-02-28-preview.md) |2015-02-28-Preview |Vorschau |
| [Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn832684.aspx) |2015-08-19 |Allgemein verfügbar |

Für die REST-APIs muss bei jedem Aufruf die `api-version` einbezogen werden. Dies erleichtert das Ansprechen einer bestimmten Version, z. B. einer Vorschau-API. Das folgende Beispiel veranschaulicht, wie der Parameter `api-version` angegeben wird:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2016-09-01

> [!NOTE]
> Obwohl jede Anforderung über eine `api-version` verfügt, sollten Sie für alle API-Anforderungen die gleiche Version verwenden. Dies gilt insbesondere dann, wenn neue API-Versionen Attribute oder Vorgänge einführen, die von früheren Versionen nicht erkannt werden. Das Mischen von API-Versionen kann unerwartete Konsequenzen haben und sollte vermieden werden.
>
> Die Dienst-REST-API und Verwaltungs-REST-API werden voneinander unabhängig versioniert. Jede Ähnlichkeit von Versionsnummern ist zufällig.
>
>

Allgemein verfügbare APIs (oder GA) können in der Produktion verwendet werden und unterliegen den Vereinbarungen zum Servicelevel von Azure. Vorschauversionen weisen experimentelle Funktionen auf, die nicht immer zu GA-Versionen migriert werden. **Wir raten dringend davon ab, Vorschau-APIs in Produktionsanwendungen einzusetzen.**

## <a name="sdk-version-roadmap"></a>SDK-Versionsroadmap 
Jede Version des .NET SDK spricht eine bestimmte Version der Dienst-REST-API an. Funktionen werden zunächst in der REST-API eingeführt und dann in das SDK implementiert.

Das .NET SDK ist jetzt allgemein verfügbar, und derzeit wird bereits die nächste Version vorbereitet. In der folgenden Tabelle sind zukünftige Versionen des SDK aufgeführt, sodass Sie eine Vorstellung haben, was als Nächstes kommt.

| .NET SDK-Version | REST-API-Version | Features | ETA |
| --- | --- | --- | --- |
| 1.1 |2015-02-28 |Lucene-Abfragesyntax |Februar 2016 |
| 2.0-Vorschau |2015-02-28-Preview |Benutzerdefinierte Analyse, Azure-Blob- und Tabellenindexer, Feldzuordnungen, ETags |August 2016 |
| 3.x |2016-09-01 |Identisch mit 2.0-Vorschau, ohne Unterstützung von Blobindexern für CSV- und JSON-Dateien |November 2016 |

## <a name="about-preview-and-generally-available-versions"></a>Informationen zu Vorschau- und allgemein verfügbaren Versionen
Azure Search bietet immer zuerst Vorabversionen experimenteller Funktionen über die REST-API, dann über Vorabversionen von .NET SDK.

Previewfunktionen werden nicht unbedingt zu einer GA-Version migriert. Während bei Funktionen in einer GA-Version vorausgesetzt wird, dass sie stabil sind und es unwahrscheinlich ist, dass sie geändert werden – mit Ausnahme von kleinen abwärtskompatiblen Korrekturen und Verbesserungen – stehen Previewfunktionen für Tests und Experimente zur Verfügung, mit dem Ziel, Feedback zu Entwurf und Implementierung der Funktionen zu sammeln.

Aber weil Änderungen bei Previewfunktionen vorbehalten sind, raten wir davon ab, Produktionscode zu schreiben, der von Vorschauversionen abhängig ist. Wenn Sie eine ältere Vorschauversion verwenden, sollten Sie zur allgemein verfügbaren Version (GA) migrieren.

Für das .NET SDK: Anleitung zur Codemigration finden Sie unter [Upgrade auf Version 1.1 des Azure Search .NET SDK](search-dotnet-sdk-migration.md).

Allgemeine Verfügbarkeit bedeutet, dass Azure Search jetzt der Vereinbarung zum Servicelevel (SLA) unterliegt. Die SLA finden Sie unter [SLA für Search](https://azure.microsoft.com/support/legal/sla/search/v1_0/).



<!--HONumber=Nov16_HO3-->


