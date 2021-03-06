---
title: Bewertungsprofile (Azure Search-REST-API, Version 2015-02-28-Preview) | Microsoft Docs
description: "Azure Search ist ein gehosteter Cloudsuchdienst, der die Optimierung der Rangfolge von Ergebnissen basierend auf benutzerdefinierten Bewertungsprofilen unterstützt."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: bfd62649-13d7-40b3-a8fa-85521a15084d
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.author: heidist
ms.date: 10/27/2016
translationtype: Human Translation
ms.sourcegitcommit: fc2f30569acc49dd383ba230271989eca8a14423
ms.openlocfilehash: 0488d5a7ea9758c6c08cd1886e5fa2b04b4dc85e

---

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Bewertungsprofile (Azure Search REST-API Version 2015-02-28-Preview)
> [!NOTE]
> Dieser Artikel beschreibt die Bewertungsprofile in [2015-02-28-Preview](search-api-2015-02-28-preview.md). Zurzeit besteht kein Unterschied zwischen der Version `2016-09-01`, die auf [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) dokumentiert ist, und der hier beschriebenen Version `2015-02-28-Preview`. Wir stellen dieses Dokument jedoch trotzdem bereit, damit die gesamte API dokumentiert ist.
>
>

## <a name="overview"></a>Übersicht
Der Begriff "Bewertung" verweist auf die Berechnung einer Suchbewertung für jedes in den Suchergebnissen zurückgegebene Element. Die Bewertung ist ein Indikator für die Relevanz eines Elements im Kontext des aktuellen Suchvorgangs. Je höher die Bewertung, desto relevanter das Element. In den Suchergebnissen ist den Elementen eine absteigende Reihenfolge zugeordnet, die auf den für die einzelnen Elemente berechneten Suchbewertungen basiert.

Azure Suche verwendet die Standardbewertung zum Berechnen eines Ursprungswerts. Sie können die Berechnung jedoch über ein Bewertungsprofil anpassen. Bewertungsprofile bieten Ihnen mehr Kontrolle über die Rangfolge der Elemente in Suchergebnissen. Sie können beispielsweise Elemente auf Basis ihres Umsatzpotentials optimieren, neuere Elemente hochstufen oder auch Elemente fördern, die sich bereits zu lange im Lager befinden.

Ein Bewertungsprofil ist Teil der Indexdefinition und besteht aus Feldern, Funktionen und Parametern.

Das folgende Beispiel zeigt, wie ein Bewertungsprofil aussieht. Dieses einfache Profil hat die Bezeichnung "geo". In diesem Fall werden Elemente verstärkt, die den Suchbegriff im Feld `hotelName` enthalten. Darüber hinaus wird die `distance`-Funktion verwendet, um Elemente zu bevorzugen, die sich im Umkreis von zehn Kilometern um die aktuelle Position befinden. Wenn nach dem Begriff "inn" gesucht wird und "inn" Teil des Hotelnamens ist, werden die Dokumente in den Suchergebnissen weiter oben angezeigt, die Hotels mit "inn" im Namen enthalten.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Um dieses Bewertungsprofil zu verwenden, muss die Abfrage so formuliert werden, dass das Profil in der Abfragezeichenfolge angegeben wird. Beachten Sie in der folgenden Abfrage den Abfrageparameter `scoringProfile=geo` in der Anforderung.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Diese Abfrage sucht nach dem Begriff "inn" und übergibt die aktuelle Position. Beachten Sie, dass diese Abfrage weitere Parameter einbezieht, z. B. `scoringParameter`. Eine Beschreibung der Abfrageparameter finden Sie unter [Durchsuchen von Dokumenten (Azure Search-API)](search-api-2015-02-28-preview.md#SearchDocs).

Klicken Sie auf [Beispiel](#example) , um ein ausführlicheres Beispiel für ein Bewertungsprofil zu überprüfen.

## <a name="what-is-default-scoring"></a>Standardbewertung
Bei der Bewertung wird eine Suchbewertung für jedes Element in einem nach Rangfolge sortierten Resultset berechnet. Jedes Element in einem Resultset wird einer Suchbewertung zugeordnet und anschließend wird eine absteigende Rangfolge zugewiesen. Die Elemente mit den höchsten Bewertungen werden an die Anwendung zurückgegeben. Standardmäßig werden die ersten 50 Elemente zurückgegeben, Sie können jedoch den `$top` -Parameter verwenden, um eine kleinere oder größere Anzahl von Elementen (bis zu 1000 in einer einzelnen Antwort) zurückzugeben.

Standardmäßig wird eine Suchbewertung auf Basis der statistischen Eigenschaften der Daten und der Abfrage berechnet. Azure Search findet Dokumente, die die Suchbegriffe in der Abfragezeichenfolge enthalten (einige oder alle in Abhängigkeit von `searchMode`), wobei Dokumente bevorzugt werden, die viele Instanzen des Suchbegriffs enthalten. Die Suchbewertung fällt sogar noch höher aus, wenn der Begriff nur selten im Datenhauptteil, jedoch innerhalb des Dokuments häufig vorkommt. Die Grundlage für diesen Ansatz zur Berechnung der Relevanz wird als TF-IDF (Term Frequency-Inverse Document Frequency, Vorkommenshäufigkeit-Inverse Dokumenthäufigkeit) bezeichnet.

Wenn keine benutzerdefinierte Sortierung erfolgt, werden die Ergebnisse nach Suchbewertung sortiert, bevor sie an die aufrufende Anwendung zurückgegeben werden. Wenn `$top` nicht angegeben ist, werden die 50 Elemente mit der höchsten Suchbewertung zurückgegeben.

Suchbewertungswerte können in einem Resultset wiederholt vorkommen. Es können z. B. 10 Elemente mit einer Bewertung von 1,2, 20 Elemente mit einer Bewertung von 1,0 und 20 Elemente mit einer Bewertung von 0,5 vorliegen. Wenn mehrere Treffer dieselbe Suchbewertung aufweisen, ist die Sortierung von Elementen mit gleicher Bewertung nicht definiert und somit auch nicht stabil. Führen Sie die Abfrage erneut aus, um zu sehen, wie sich die Position der Elemente ändert. Wenn zwei Elemente mit identischer Bewertung vorliegen, kann nicht garantiert werden, welches Element zuerst angezeigt wird.

## <a name="when-to-use-custom-scoring"></a>Verwenden der benutzerdefinierten Bewertung
Wenn das standardmäßige Rangfolgeverhalten Ihre Geschäftsziele nicht ausreichend unterstützt, sollten Sie ein oder mehrere Bewertungsprofile erstellen. Sie können beispielsweise festlegen, dass neu hinzugefügte Elemente hinsichtlich der Suchrelevanz bevorzugt werden sollen. Möglicherweise verfügen Sie auch über ein Feld, das die Gewinnspanne enthält, oder ein anderes Feld, das das Umsatzpotenzial angibt. Die Verstärkung von Treffern, die Ihrem Unternehmen Vorteile verschaffen, kann eine wichtige Rolle bei der Entscheidung zur Verwendung von Bewertungsprofilen spielen.

Die relevanzbasiert Sortierung wird ebenfalls über Bewertungsprofile implementiert. Betrachten Sie Suchergebnisseiten, die Sie in der Vergangenheit verwendet haben, mit denen Sie nach Preis, Datum, Bewertung oder Relevanz sortieren können. In Azure Search wird der Faktor "Relevanz" durch Bewertungsprofile gesteuert. Die Definition von Relevanz wird von Ihnen gesteuert und basiert auf Ihren Geschäftszielen und der Suchfunktionalität, die Sie bereitstellen möchten.

<a name="example"></a>

## <a name="example"></a>Beispiel
Wie bereits erwähnt, wird die benutzerdefinierte  Bewertung über Bewertungsprofile implementiert, die in einem Indexschema definiert sind.

In diesem Beispiel wird das Schema für einen Index mit zwei Bewertungsprofilen (`boostGenre`, `newAndHighlyRated`) gezeigt. Jede Abfrage für diesen Index, die eines der Profile als Abfrageparameter enthält, verwendet das Profil zum Bewerten des Resultsets.

[Testen Sie dieses Beispiel](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Workflow
Um ein benutzerdefiniertes Bewertungsverhalten zu implementieren, fügen Sie dem Schema, das den Index definiert, ein Bewertungsprofil hinzu. Sie können in einem Index bis zu 16 Bewertungsprofile verwenden (siehe [Diensteinschränkungen](search-limits-quotas-capacity.md)), aber Sie können in einer Abfrage jeweils nur ein Profil angeben.

Beginnen Sie mit der in diesem Thema angegebenen [Vorlage](#bkmk_template) .

Geben Sie einen Namen ein. Bewertungsprofile sind optional, wenn Sie jedoch ein Profil hinzufügen, ist der Name erforderlich. Achten Sie darauf, dass Sie die Namenskonventionen für Felder einhalten (mit einem Buchstaben beginnen, Sonderzeichen und reservierte Wörter vermeiden). Weitere Informationen finden Sie unter [Benennungsregeln](http://msdn.microsoft.com/library/azure/dn857353.aspx) .

Der Hauptteil des Bewertungsprofils wird aus gewichteten Feldern und Funktionen erstellt.

### <a name="weights"></a>Weights
Die `weights`-Eigenschaft eines Bewertungsprofils gibt die Name-Wert-Paare an, die einem Feld eine relative Gewichtung zuordnen. In diesem [Beispiel](#example) werden die Felder „albumTitle“, „genre“ und „artistName“ entsprechend um 1,5, 5 und 2 verstärkt. Warum wird "genre" so viel stärker als die anderen Felder erhöht? Wenn die Suche über relativ homogene Daten durchgeführt wird (wie bei "genre" in `musicstoreindex`), ist bei den relativen Gewichtungen möglicherweise eine größere Varianz erforderlich. In `musicstoreindex` wird "rock" z. B. sowohl für "genre" als auch für identisch formulierte Genrebeschreibungen angezeigt. Wenn "genre" schwerer wiegen soll als die Genrebeschreibung, dann benötigt das Feld "genre" eine viel höhere relative Gewichtung.

### <a name="functions"></a>Functions
Funktionen werden verwendet, wenn für bestimmte Kontexte zusätzliche Berechnungen erforderlich sind. Gültige Funktionstypen sind `freshness`, `magnitude`, `distance` und `tag`. Jede Funktion verfügt über Parameter, die für sie eindeutig sind.

* `freshness` sollte verwendet werden, wenn Sie Elemente in Abhängigkeit davon verstärken möchten, wie neu oder alt diese sind. Diese Funktion kann nur mit Datetime-Feldern (`Edm.DataTimeOffset`) verwendet werden. Beachten Sie, dass das Attribut `boostingDuration` nur mit der Funktion „freshness“ verwendet wird.
* `magnitude` sollte verwendet werden, wenn die Verstärkung basierend auf der Höhe eines numerischen Werts erfolgen soll. Szenarien, die diese Funktion erforderlich machen, umfassen die Verstärkung nach Gewinnspanne, Höchstpreis, Mindestpreis oder Downloadanzahl.  Sie können den Bereich (absteigend) umkehren, wenn Sie das umgekehrte Muster anwenden möchten (z.B. um preiswerteren Elemente eine höhere Relevanz zuzuordnen als teureren Elementen). Legen Sie bei einer Preisspanne zwischen 100 € und 1 € `boostingRangeStart` auf 100 und `boostingRangeEnd` auf 1 fest, um preiswertere Artikeln zu fördern. Diese Funktion kann nur mit Double- und Integer-Feldern verwendet werden.
* `distance` sollte verwendet werden, wenn die Verstärkung entsprechend der Nähe oder geografischen Lage erfolgen soll. Diese Funktion kann nur mit `Edm.GeographyPoint` -Feldern verwendet werden.
* `tag` sollte verwendet werden, wenn die Verstärkung nach gemeinsamen Tags in Dokumenten und Suchabfragen erfolgen soll. Diese Funktion kann nur mit `Edm.String`- und `Collection(Edm.String)`-Feldern verwendet werden.

#### <a name="rules-for-using-functions"></a>Regeln für die Verwendung von Funktionen
* Der Funktionstyp (freshness, magnitude, distance, tag) muss in Kleinbuchstaben angegeben werden.
* Funktionen dürfen nicht Null sein oder leere Werte enthalten. Insbesondere beim Einbeziehen von Feldnamen muss ein Wert festgelegt werden.
* Funktionen können nur auf filterbare Felder angewendet werden. Weitere Informationen zu filterbaren Feldern finden Sie unter [Erstellen des Index](search-api-2015-02-28-preview.md#CreateIndex).
* Funktionen können nur auf Felder angewendet werden, die in der Felderauflistung für einen Index definiert sind.

Nachdem der Index definiert wurde, erstellen Sie den Index durch Hochladen des Indexschemas, gefolgt von Dokumenten. Anweisungen zu diesen Vorgängen finden Sie unter [Erstellen des Index](search-api-2015-02-28-preview.md#CreateIndex) und [Dokumente hinzufügen oder aktualisieren](search-api-2015-02-28-preview.md#AddOrUpdateDocuments). Nachdem der Index erstellt wurde, sollten Sie über ein funktionsfähiges Bewertungsprofil verfügen, das mit Ihren Suchdaten arbeitet.

<a name="bkmk_template"></a>

## <a name="template"></a>Vorlage
In diesem Abschnitt wird die Syntax und die Vorlage für die Bewertungsprofile veranschaulicht. Beschreibungen zu den Attributen finden Sie im nächsten Abschnitt unter [Indexattributreferenz](#bkmk_indexref) .

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
            ...
          }
        },
        "functions": (optional) [
          {
            "type": "magnitude | freshness | distance | tag",
            "boost": # (positive number used as multiplier for raw score != 1),
            "fieldName": "...",
            "interpolation": "constant | linear (default) | quadratic | logarithmic",

            "magnitude": {
              "boostingRangeStart": #,
              "boostingRangeEnd": #,
              "constantBoostBeyondRange": true | false (default)
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>

## <a name="scoring-profile-property-reference"></a>Bewertungsprofil-Eigenschaftenreferenz
> [!NOTE]
> Eine Bewertungsfunktion kann nur auf filterbare Felder angewendet werden.
>
>

| Eigenschaft | Beschreibung |
| --- | --- |
| `name` |Erforderlich. Dies ist der Name des Bewertungsprofils. Er folgt denselben Benennungskonventionen, die für Felder gelten. Er muss mit einem Buchstaben beginnen, darf keine Punkte, Doppelpunkte oder @-Symbole enthalten und darf nicht mit dem Begriff „azureSearch“ beginnen (Groß-/Kleinschreibung wird berücksichtigt). |
| `text` |Enthält die Weights-Eigenschaft. |
| `weights` |Optional. Ein Name-Wert-Paar, das einen Feldnamen und eine relative Gewichtung angibt. Die relative Gewichtung muss eine positive ganze oder Gleitkommazahl sein. Sie können den Feldnamen ohne eine entsprechende Gewichtung angeben. Gewichtungen werden dazu verwendet, um die Bedeutung der Felder untereinander anzugeben. |
| `functions` |Optional. Beachten Sie, dass eine Bewertungsfunktion nur auf Felder angewendet werden kann, die filterbar sind. |
| `type` |Erforderlich für Bewertungsfunktionen. Gibt den Typ der zu verwendenden Funktion an. Gültige Werte sind `magnitude`, `freshness`, `distance` und `tag`. Sie können in jedem Bewertungsprofil mehrere Funktionen einbeziehen. Der Funktionsname muss in Kleinbuchstaben angegeben werden. |
| `boost` |Erforderlich für Bewertungsfunktionen. Eine positive Zahl, die als Multiplikator für die unformatierte Bewertung verwendet wird. Sie darf nicht gleich 1 sein. |
| `fieldName` |Erforderlich für Bewertungsfunktionen. Eine Bewertungsfunktion kann nur auf Felder angewendet werden, die Teil der Feldauflistung für den Index und filterbar sind. Darüber hinaus führt jeder Funktionstyp zusätzliche Einschränkungen ein ("frehsness" wird für Datetime-Felder, "magnitude" für Integer- oder Double-Felder, "distance" für Location-Felder und "tag" für Zeichenfolgen- oder Zeichenfolgenauflistungsfelder verwendet). Sie können pro Funktionsdefinition nur ein einzelnes Feld angeben. Um z. B. "magnitude" zweimal in demselben Profil verwenden zu können, müssen Sie zwei Definitionen für "magnitude", eine für jedes Feld, einbeziehen. |
| `interpolation` |Erforderlich für Bewertungsfunktionen. Definiert die Steigung, für die die Bewertungsverstärkung vom Anfang bis zum Ende des Bereichs zunimmt. Gültige Werte sind `linear` (Standard), `constant`, `quadratic` und `logarithmic`. Details finden Sie unter [Festlegen von Interpolationen](#bkmk_interpolation) . |
| `magnitude` |Die Bewertungsfunktion für die Größe wird dazu verwendet, um Rangfolgen auf Basis des Wertebereichs für ein numerisches Feld zu ändern. Einige der gängigsten Nutzungsbeispiele sind:<ul><li> Sternbewertungen: Ändern Sie die Bewertung basierend auf dem Wert innerhalb des Felds für die „Sternbewertung“. Wenn zwei Elemente relevant sind, wird das Element mit der höheren Bewertung zuerst angezeigt.</li><li>Gewinnspanne: Wenn zwei Dokumente relevant sind, möchte ein Einzelhändler möglicherweise zuerst Dokumente verstärken, die eine höhere Gewinnspanne aufweisen.</li><li>Anzahl der Klicks: Für Anwendungen, bei denen das Klicken über Aktionen mit Produkten oder Seiten nachverfolgt wird, könnten Sie die „Größe“ verwenden, um Elemente zu verstärken, die in der Regel den meisten Datenverkehr aufweisen.</li><li>Anzahl der Downloads: Für Anwendungen, die Downloads nachverfolgen, können Sie mit der Größenfunktion die Elemente verstärken, die am meisten heruntergeladen wurden.</li></ul> |
| `magnitude:boostingRangeStart` |Legt den Anfangswert des Bereichs fest, über den die Größe bewertet wird. Der Wert muss vom Typ „Integer“ oder „Gleitkomma“ sein. Für Sternbewertungen von 1 bis 4 wäre dies die 1. Für Gewinnspannen von über 50 % wäre dies die 50. |
| `magnitude:boostingRangeEnd` |Legt den Endwert des Bereichs fest, über den die Größe bewertet wird. Der Wert muss vom Typ „Integer“ oder „Gleitkomma“ sein. Für Sternbewertungen von 1 bis 4 wäre dies die 4. |
| `magnitude:constantBoostBeyondRange` |Gültige Werte sind "true" oder "false" (Standard). Bei "true" wird die vollständige Verstärkung weiterhin auf Dokumente angewendet, die einen Wert für das Zielfeld aufweisen, der über dem oberen Ende des Bereichs liegt. Bei "false" wird die Verstärkung dieser Funktion nicht auf Dokumente angewendet, die einen Wert für das Zielfeld aufweisen, der außerhalb des Bereichs liegt. |
| `freshness` |Die Bewertungsfunktion für die Aktualität wird dazu verwendet, um Rangfolgenbewertungen für Elemente auf Basis von Werten in DateTimeOffset-Feldern zu ändern. Ein Element mit einem aktuelleren Datum kann z. B. höher als ältere Elemente eingestuft werden.  (Beachten Sie, dass z. B. Kalenderereignisse mit in der Zukunft liegenden Daten so eingestuft werden können, dass Ereignisse mit geringerem Abstand zur Gegenwart höher als Ereignisse eingestuft werden, die weiter in der Zukunft liegen.) Im aktuellen Service Release wird ein Ende des Bereichs auf die aktuelle Zeit festgelegt. Das andere Ende ist ein Zeitpunkt in der Vergangenheit, der auf `boostingDuration` basiert. Um einen Bereich von Zeitpunkten in der Zukunft zu verstärken, verwenden Sie einen negativen Wert für `boostingDuration`. Die Rate, mit der die Verstärkung von einem maximalen und minimalen Bereich wechselt, wird durch die Interpolation bestimmt, die auf das Bewertungsprofil angewendet wird (siehe folgende Abbildung). Wählen Sie zum Umkehren des angewendeten Verstärkungsfaktors einen Verstärkungsfaktor, der kleiner ist als 1. |
| `freshness:boostingDuration` |Legt eine Ablaufdauer fest, nach der die Verstärkung für ein bestimmtes Dokument beendet wird. Informationen zur Syntax und Beispiele finden Sie im folgenden Abschnitt unter [Festlegen von „boostingDuration“](#bkmk_boostdur). |
| `distance` |Die Bewertungsfunktion für den Abstand wird dazu verwendet, um auf Basis des Abstands relativ zu einem geografischen Standort Einfluss auf die Bewertung von Dokumenten zu nehmen. Der Referenzstandort wird als Teil der Abfrage in einem Parameter (mithilfe des Abfrageparameters `scoringParameter` ) als „lon,lat“-Argument (Längengrad, Breitengrad) angegeben. |
| `distance:referencePointParameter` |Ein in Abfragen zu übergebender Parameter, der als Referenzstandort verwendet wird. scoringParameter ist ein Abfrageparameter; "scoringParameter" ist ein Abfrageparameter. Beschreibungen zu den Abfrageparametern finden Sie unter [Dokumente durchsuchen (REST-API in Azure Search-Dienst)](search-api-2015-02-28-preview.md#SearchDocs) . |
| `distance:boostingDistance` |Dies muss eine ganze Zahl sein. Legt den Abstand in Kilometern vom Referenzstandort fest, an dem der Verstärkungsbereich endet. |
| `tag` |Die Bewertungsfunktion "tag" wird verwendet, um die Bewertung von Dokumenten auf Grundlage der Tags in Dokumenten und Suchabfragen zu beeinflussen. Dokumente, die dieselben Tags wie die Suchabfrage enthalten, werden verstärkt. Die Tags für die Suchabfrage dienen als Bewertungsparameter in jeder Suchanforderung (mithilfe des `scoringParameter` -Abfrageparameters). |
| `tag:tagsParameter` |Ein in Abfragen zu übergebender Parameter, der Tags für eine bestimmten Anforderung angibt. `scoringParameter` ist ein Abfrageparameter. Beschreibungen zu den Abfrageparametern finden Sie unter [Dokumente durchsuchen (REST-API in Azure Search-Dienst)](search-api-2015-02-28-preview.md#SearchDocs) . |
| `functionAggregation` |Optional. Gilt nur, wenn Funktionen angegeben sind. Gültige Werte sind `sum` (Standard), `average`, `minimum`, `maximum` und `firstMatching`. Eine Suchbewertung entspricht einem einzelnen Wert, der aus mehreren Variablen berechnet wird, einschließlich mehrerer Funktionen. Dieses Attribut gibt an, wie die Verstärkungen aller Funktionen zu einer einzelnen Gesamtverstärkung zusammengefasst werden, die dann auf die Basisdokumentbewertung angewendet wird. Die Basisbewertung basiert auf dem tf-idf-Wert, der aus dem Dokument und der Suchabfrage berechnet wird. |
| `defaultScoringProfile` |Beim Ausführen einer Suchanforderung wird die Standardbewertung verwendet (nur tf-idf), wenn kein Bewertungsprofil angegeben ist. Hier kann ein Standardname für das Bewertungsprofil festgelegt werden. Dies führt dazu, dass Azure Search dieses Profil verwendet, wenn in der Suchanforderung kein bestimmtes Profil angegeben ist.  |

<a name="bkmk_interpolation"></a>

## <a name="set-interpolations"></a>Festlegen von Interpolationen
Interpolationen ermöglichen Ihnen die Definition der Steigung, in der die Bewertungsoptimierung vom Anfang bis zum Ende des Bereichs zunimmt. Die folgenden Interpolationen können verwendet werden:

* `Linear`: Für Elemente, die sich innerhalb des maximalen und minimalen Bereichs befinden, erfolgt die auf das Element angewendete Verstärkung mit konstant abnehmender Stärke. "Linear" ist die Standardinterpolation für ein Bewertungsprofil.
* `Constant`: Für Elemente, die sich innerhalb des Anfangs- und Endbereichs befinden, wird eine konstante Verstärkung auf die Rangfolgeergebnisse angewendet.
* `Quadratic`: Im Vergleich zur linearen Interpolation, die eine konstant abnehmende Verstärkung aufweist, erfolgt die Abnahme bei „Quadratic“ anfänglich mit geringerer Geschwindigkeit, während zum Bereichsende hin die Abnahme in viel größeren Schritten erfolgt. Diese Interpolationsoption ist in Tag-Bewertungsfunktionen nicht zulässig.
* `Logarithmic`: Im Vergleich zur linearen Interpolation, die eine konstant abnehmende Verstärkung aufweist, erfolgt die Abnahme bei „Logarithmic“ anfänglich mit höherer Geschwindigkeit, während zum Bereichsende hin die Abnahme in viel kleineren Schritten erfolgt. Diese Interpolationsoption ist in Tag-Bewertungsfunktionen nicht zulässig.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>

## <a name="set-boostingduration"></a>Festlegen von boostingDuration
`boostingDuration` ist ein Attribut der Funktion "freshness". Sie können damit eine Ablaufdauer festlegen, nach der die Verstärkung für ein bestimmtes Dokument beendet wird. Um beispielsweise eine Produktlinie oder Marke für einen zehntägigen Werbezeitraum zu verstärken, können Sie den zehntägigen Zeitraum für diese Dokumente z. B. als "P10D" angeben. Oder geben Sie "-P7D" an, um bevorstehende Ereignisse in der nächsten Woche zu verstärken.

`boostingDuration` muss als XSD-Wert "dayTimeDuration" formatiert sein (eine eingeschränkte Teilmenge eines ISO 8601-Zeitdauerwerts). Das Muster hierfür lautet wie folgt: `[-]P[nD][T[nH][nM][nS]]`.

Die folgende Tabelle enthält einige Beispiele.

| Duration | boostingDuration |
| --- | --- |
| 1 Tag |"P1D" |
| 2 Tage und 12 Stunden |"P2DT12H" |
| 15 Minuten |"PT15M" |
| 30 Tage, 5 Stunden, 10 Minuten und 6,334 Sekunden |"P30DT5H10M6.334S" |

Weitere Beispiele finden Sie unter [XML-Schema: Datentypen (W3.org-Website)](http://www.w3.org/TR/xmlschema11-2/).

**Siehe auch**
[REST-API für Azure Search-Dienst](http://msdn.microsoft.com/library/azure/dn798935.aspx) auf MSDN <br/>
[Erstellen des Index (Azure Search-API)](http://msdn.microsoft.com/library/azure/dn798941.aspx) auf MSDN<br/>
[Hinzufügen eines Bewertungsprofils zu einem Suchindex](http://msdn.microsoft.com/library/azure/dn798928.aspx) auf MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png



<!--HONumber=Nov16_HO3-->


