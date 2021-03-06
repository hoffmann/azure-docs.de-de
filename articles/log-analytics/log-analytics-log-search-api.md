---
title: "Log Analytics-REST-API für die Protokollsuche | Microsoft Docs"
description: "Dieser Artikel enthält ein allgemeines Tutorial, in dem beschrieben wird, wie Sie die Log Analytics-REST-API für die Suche in der Operations Management Suite (OMS) nutzen können. Außerdem werden Beispiele zur Verwendung der Befehle genannt."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: b4e9ebe8-80f0-418e-a855-de7954668df7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 81dd7d9dc799f6f4c0dd54a12409724c182a0349


---
# <a name="log-analytics-log-search-rest-api"></a>Log Analytics-REST-API für die Protokollsuche
Dieser Artikel enthält ein allgemeines Tutorial, in dem beschrieben wird, wie Sie die Log Analytics-REST-API für die Suche in der Operations Management Suite (OMS) nutzen können. Außerdem werden Beispiele zur Verwendung der Befehle genannt. Einige Beispiele in diesem Artikel verweisen auf Operational Insights – dies ist der Name der Vorgängerversion von Log Analytics.

## <a name="overview-of-the-log-search-rest-api"></a>Übersicht über die Protokollsuch-REST-API
Die REST-API für die Log Analytics-Suche ist RESTful. Der Zugriff darauf erfolgt über die Azure Resource Manager-API. In diesem Dokument finden Sie Beispiele, in denen über [ARMClient](https://github.com/projectkudu/ARMClient) auf die API zugegriffen wird. Dies ist ein Open Source-Befehlszeilentool, das den Aufruf der Azure Resource Manager-API vereinfacht. Die Verwendung von ARMClient und PowerShell ist eine von vielen Möglichkeiten, auf die Protokollsuch-API von Log Analytics zuzugreifen. Eine weitere Option besteht in der Verwendung des Azure PowerShell-Moduls für Operational Insights, das Cmdlets für den Zugriff auf die Suche enthält. Mit diesen Tools können Sie die RESTful-API von Azure Resource Manager für Aufrufe an OMS-Arbeitsbereiche nutzen und darin Suchbefehle ausführen. Die API wird Ihnen Suchergebnisse im JSON-Format ausgeben, und Sie können die Suchergebnisse programmgesteuert auf viele verschiedene Arten verwenden.

Der Azure Resource Manager kann über eine [Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) oder eine [REST-API](https://msdn.microsoft.com/library/azure/mt163658.aspx) verwendet werden. Überprüfen Sie die verlinkten Webseiten, um mehr zu erfahren.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Allgemeines Tutorial zur Log Analytics-REST-API für die Protokollsuche
### <a name="to-use-the-arm-client"></a>Verwendung von ARMClient
1. Installieren Sie [Chocolatey](https://chocolatey.org/), ein Open Source-Paket-Manager für Windows. Öffnen Sie eine Eingabeaufforderung als Administrator, und führen Sie den folgenden Befehl aus:
   
    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. Installieren Sie ARMClient, indem Sie den folgenden Befehl ausführen:
   
    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Eine einfache Suche mit ARMClient
1. Melden Sie sich in Ihrem Microsoft- oder Organisations-ID-Konto an:
   
    ```
    armclient login
    ```
   
    Eine erfolgreiche Anmeldung listet alle Abonnements auf, die mit dem angegebenen Konto verknüpft sind:
   
    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. Rufen Sie die Operations Management Suite-Arbeitsbereiche auf:
   
    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```
   
    Ein erfolgreicher Get-Aufruf gibt alle Arbeitsbereiche aus, die mit dem Abonnement verknüpft sind:
   
    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Erstellen Sie Ihre Suchvariable:
   
    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Führen Sie mit der neuen Suchvariablen eine Suche durch:
   
    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Referenzbeispiele für die Log Analytics-REST-API für die Suche
Die folgenden Beispiele zeigen Ihnen, wie Sie die Search-API verwenden können.

### <a name="search---actionread"></a>Search – Aktion/Lesen
**Beispiel-URL:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Anforderung:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
Die folgende Tabelle beschreibt die verfügbaren Eigenschaften.

| **Eigenschaft** | **Beschreibung** |
| --- | --- |
| top |Die maximale Anzahl der zurückzugebenden Ergebnisse. |
| highlight |Enthält Pre- und Post-Parameter, wird in der Regel für die Hervorhebung von übereinstimmenden Feldern verwendet. |
| pre |Steht der angegebenen Zeichenfolge in den entsprechenden Feldern als Präfix voran. |
| post |Fügt die angegebene Zeichenfolge an die entsprechenden Felder an. |
| query |Die Suchabfrage zum Erfassen und Zurückgeben von Ergebnissen. |
| start |Der Anfang des Zeitfensters, in dem die Ergebnisse gesucht werden sollen. |
| end |Das Ende des Zeitfensters, in dem die Ergebnisse gesucht werden sollen. |

**Antwort:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Search/{ID} – Aktion/Lesen
**Fordern Sie den Inhalt einer gespeicherten Suche an:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> Wenn die Suche den Status "Ausstehend" zurückgibt, können die aktualisierten Ergebnisse über diese API aufgerufen werden. Nach sechs Minuten wird das Ergebnis der Suche aus dem Cache gelöscht, und es wird „HTTP Fehlend“ zurückgegeben. Wenn die anfängliche Suchanforderung sofort den Status „Erfolgreich“ zurückgibt, wird sie nicht zum Cache hinzugefügt. Dadurch gibt diese API bei einer Abfrage „HTTP Fehlend“ zurück. Der Inhalt eines „HTTP 200“-Ergebnisses wird im gleichen Format wie die ursprüngliche Suchanforderung erstellt, nur mit aktualisierten Werten.
> 
> 

### <a name="saved-searches---rest-only"></a>Gespeicherte Suchvorgänge - nur REST
**Fordern Sie die Liste der gespeicherten Suchvorgänge an:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Unterstützte Methoden: GET PUT DELETE

Unterstützte Erfassungsmethoden: GET

Die folgende Tabelle beschreibt die verfügbaren Eigenschaften.

| Eigenschaft | Beschreibung |
| --- | --- |
| ID |Der eindeutige Bezeichner. |
| ETag |**Erforderlich für Patch**. Wird bei jedem Schreibvorgang vom Server aktualisiert. Zum Aktualisieren muss der Wert mit dem aktuellen gespeicherten Wert übereinstimmen oder "*" lauten. 409 für alte/ungültige Werte zurückgegeben. |
| properties.query |**Erforderlich**. Die Suchabfrage |
| properties.displayName |**Erforderlich**. Der benutzerdefinierte Anzeigename der Abfrage. Bei Modellierung als Azure-Ressource wäre dies ein Tag. |
| properties.category |**Erforderlich**. Die benutzerdefinierte Kategorie der Abfrage. Bei Modellierung als Azure-Ressource wäre dies ein Tag. |

> [!NOTE]
> Beim Abrufen gespeicherter Suchvorgänge in einem Arbeitsbereich gibt die Log Analytics-Such-API zurzeit vom Benutzer erstellte gespeicherte Suchvorgänge zurück. Momentan gibt die API keine von Lösungen bereitgestellten gespeicherten Suchvorgänge zurück. Diese Funktionalität wird zu einem späteren Zeitpunkt hinzugefügt.
> 
> 

### <a name="create-saved-searches"></a>Erstellen gespeicherter Suchvorgänge
**Anforderung:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Löschen gespeicherter Suchvorgänge
**Anforderung:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Aktualisieren gespeicherter Suchvorgänge
 **Anforderung:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metadaten: nur JSON
Dies ist eine Möglichkeit, die Felder für alle Protokolltypen für die Daten anzuzeigen, die im Arbeitsbereich gesammelt wurden. Wenn Sie beispielsweise wissen möchten, ob der Ereignistyp ein Feld mit dem Namen "Computer" aufweist, können Sie dies unter anderem hiermit herausfinden.

**Felder anfordern:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Antwort:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

Die folgende Tabelle beschreibt die verfügbaren Eigenschaften.

| **Eigenschaft** | **Beschreibung** |
| --- | --- |
| Name |Feldname. |
| displayName |Der Anzeigename des Felds. |
| type |Der Typ des Feldwerts. |
| facetable |Kombination der aktuellen Eigenschaften "indexed", "stored" und "facet". |
| display |Aktuelle display-Eigenschaft. "True", wenn das Feld in Search angezeigt wird. |
| ownerType |Nur Typen, die zu eingebundenen IP-Adressen gehören. |

## <a name="optional-parameters"></a>Optionale Parameter
Im Folgenden werden die verfügbaren optionalen Parameter beschrieben.

### <a name="highlighting"></a>Hervorhebungen
Der Parameter "Highlight" ist ein optionaler Parameter, den Sie verwenden können, damit das Suchsubsystem einen Satz von Markern in die Antwort einbezieht.

Diese Marker stehen für den Beginn und das Ende von hervorgehobenem Text, der den in Ihrer Suchabfrage angegebenen Begriffen entspricht.
Sie können die Start- und Endmarker angeben, die bei der Suche zum Einschließen des markierten Begriffs verwendet werden.

**Beispiel für eine Suchabfrage**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Beispielergebnis:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Beachten Sie, dass das oben genannte Ergebnis eine Fehlermeldung enthält, die mit dem Präfix versehen und angefügt wurde.

## <a name="computer-groups"></a>Computergruppen
Computergruppen sind spezielle gespeicherte Suchvorgänge, die einen Satz von Computern zurückgeben.  Sie können eine Computergruppe in anderen Abfragen verwenden, um die Ergebnisse auf die Computer in dieser Gruppe zu beschränken.  Eine Computergruppe ist als gespeicherte Suche implementiert, die Markierung „Group“ weist den Wert „Computer“ auf.

Im Folgenden finden Sie eine Beispielantwort für eine Computergruppe.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
          }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Abrufen von Computergruppen
Verwenden Sie die Get-Methode mit der Gruppen-ID, um eine Computergruppe abzurufen.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Erstellen oder Aktualisieren einer Computergruppe
Verwenden Sie die Put-Methode mit der eindeutigen ID einer gespeicherten Suche, um eine neue Computergruppe zu erstellen. Wenn Sie die ID einer vorhandenen Computergruppe verwenden, wird diese ID geändert. Wenn Sie in der OMS-Konsole eine Computergruppe erstellen, wird die ID aus der Gruppe und dem Namen generiert.

Die Abfrage, die für die Gruppendefinition verwendet wird, muss einen Satz von Computern zurückgeben, damit die Gruppe ordnungsgemäß funktioniert.  Es empfiehlt sich, die Abfrage mit *| Distinct Computer* zu beenden, um sicherzustellen, dass die richtigen Daten zurückgegeben werden.

Die Definition der gespeicherten Suche muss eine Markierung namens „Group“ mit dem Wert „Computer“ aufweisen, damit die Suche als Computergruppe klassifiziert werden kann.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Löschen von Computergruppen
Verwenden Sie die Delete-Methode mit der Gruppen-ID, um eine Computergruppe zu löschen.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) , um Abfragen mithilfe von benutzerdefinierten Feldern für die Kriterien zu erstellen.




<!--HONumber=Nov16_HO3-->


