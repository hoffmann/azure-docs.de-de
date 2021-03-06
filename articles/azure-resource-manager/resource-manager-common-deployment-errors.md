---
title: "Problembehandlung bei häufigen Azure-Bereitstellungsfehlern | Microsoft Docs"
description: "Informationen zum Beheben gängiger Fehler beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: Bereitstellungsfehler, Azure-Bereitstellung, in Azure bereitstellen.
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: e2e59da29897a40f0fe538d6fe8063ae5edbaccd
ms.openlocfilehash: 4dd4e54f3e2514570ff5cbffcb926f274491cb65


---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager
In diesem Thema wird beschrieben, wie Sie einige häufige Fehler beheben, die bei der Bereitstellung in Azure auftreten können.

## <a name="two-types-of-errors"></a>Zwei Arten von Fehlern
Es gibt zwei Arten von Fehlern, die auftreten können:

* Überprüfungsfehler
* Bereitstellungsfehler

Die folgende Abbildung zeigt das Aktivitätsprotokoll für ein Abonnement. Es gibt drei Vorgänge, die in zwei Bereitstellungen aufgetreten sind. Bei der ersten Bereitstellung hat die Vorlage die Überprüfung bestanden. Beim Erstellen der Ressourcen (**Bereitstellungen schreiben**) ist jedoch ein Fehler aufgetreten. Bei der zweiten Bereitstellung ist bei der Überprüfung ein Fehler aufgetreten, weshalb der nächste Schritt (**Bereitstellungen schreiben**) nicht erfolgt ist.

![Fehlercode anzeigen](./media/resource-manager-common-deployment-errors/show-activity-log.png)

Überprüfungsfehler können bei Szenarien auftreten, für die vorab festgelegt wurde, dass sie ein Problem verursachen sollen. Zu Überprüfungsfehlern gehören Syntaxfehler in Ihrer Vorlage oder Versuche, Ressourcen bereitzustellen, die Ihr Abonnementkontingent überschreiten würden. Bereitstellungsfehler können durch Bedingungen verursacht werden, die während des Bereitstellungsprozesses auftreten. Zum Beispiel kann ein Bereitstellungsfehler infolge eines Versuchs auftreten, auf eine Ressource zuzugreifen, die parallel bereitgestellt wird.

Beide Fehlertypen geben einen Fehlercode zurück, der für die Problembehandlung für die Bereitstellung genutzt werden kann. Beide Fehlertypen werden im Aktivitätsprotokoll angezeigt. Überprüfungsfehler werden allerdings nicht im Bereitstellungsverlauf festgehalten, da die Bereitstellung tatsächlich nie gestartet wurde.


## <a name="error-codes"></a>Fehlercodes
Bei Bereitstellungsfehlern wird der Code **DeploymentFailed** zurückgegeben. Dieser Fehlercode gehört jedoch zu den allgemeinen Bereitstellungsfehlern. Der Fehlercode, mit dessen Hilfe Sie das Problem tatsächlich beheben können, befindet sich meist eine Stufe unter diesem Fehler. Die folgende Abbildung zeigt den Fehlercode **RequestDisallowedByPolicy**, der sich unter dem Bereitstellungsfehler befindet.

![Fehlercode anzeigen](./media/resource-manager-common-deployment-errors/error-code.png)

In diesem Thema werden die folgenden Fehlercodes beschrieben:

* [AccountNameInvalid](#accountnameinvalid)
* [Fehler bei der Autorisierung](#authorization-failed)
* [BadRequest](#badrequest)
* [InvalidContentLink](#invalidcontentlink)
* [InvalidTemplate](#invalidtemplate)
* [MissingSubscriptionRegistration](#noregisteredproviderfound)
* [NotFound](#notfound)
* [NoRegisteredProviderFound](#noregisteredproviderfound)
* [OperationNotAllowed](#quotaexceeded)
* [ParentResourceNotFound](#parentresourcenotfound)
* [QuotaExceeded](#quotaexceeded)
* [RequestDisallowedByPolicy](#requestdisallowedbypolicy)
* [ResourceNotFound](#notfound)
* [SkuNotAvailable](#skunotavailable)
* [StorageAccountAlreadyExists](#storagenamenotunique)
* [StorageAccountAlreadyTaken](#storagenamenotunique)

### <a name="invalidtemplate"></a>InvalidTemplate
Dieser Fehler kann aus verschiedenen Arten von Fehlern entstehen.

- Syntaxfehler

   Wenn Sie eine Fehlermeldung erhalten, die auf eine fehlerhafte Vorlagenüberprüfung hinweist, ist in Ihrer Vorlage möglicherweise ein Syntaxproblem vorhanden.

       Code=InvalidTemplate
       Message=Deployment template validation failed

   Solche Fehler können sehr leicht passieren, da Vorlagenausdrücke recht kompliziert sein können. Beispielsweise enthält die folgenden Namenszuweisung für ein Speicherkonto Klammern, drei Funktionen, drei Sätze mit Klammern, einen Satz mit einfachen Anführungszeichen und eine Eigenschaft:

       "name": "[concat('storage', uniqueString(resourceGroup().id))]",

   Wenn Sie nicht die passende Syntax bereitstellen, erzeugt die Vorlage einen Wert, der nicht Ihrer Absicht entspricht.

   Wenn Sie diese Art von Fehler erhalten, überprüfen Sie sorgfältig die Ausdruckssyntax. Verwenden Sie ggf. einen JSON-Editor, der Sie auf Syntaxfehler aufmerksam machen kann (z.B. [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) oder [Visual Studio Code](resource-manager-vs-code.md)).

- Falsche Segmentlängen

   Ein weiterer Fehler vom Typ „Ungültige Vorlage“ tritt auf, wenn der Ressourcenname nicht im richtigen Format vorliegt.

       Code=InvalidTemplate
       Message=Deployment template validation failed: 'The template resource {resource-name}'
       for type {resource-type} has incorrect segment lengths.

   Eine Ressource auf Stammebene muss im Namen ein Segment weniger aufweisen als im Ressourcentyp. Die einzelnen Segmente sind jeweils durch einen Schrägstrich getrennt. Im folgenden Beispiel weist der Typ zwei Segmente auf und der Name eins, sodass es sich hier um einen **gültigen Namen** handelt.

       {
         "type": "Microsoft.Web/serverfarms",
         "name": "myHostingPlanName",
         ...
       }

   Im nächsten Beispiel liegt dagegen **kein gültiger Name** vor, da er die gleiche Anzahl von Segmenten besitzt wie der Typ.

       {
         "type": "Microsoft.Web/serverfarms",
         "name": "appPlan/myHostingPlanName",
         ...
       }

   Bei untergeordneten Ressourcen besitzen Typ und Name die gleiche Anzahl von Segmenten. Diese Segmentanzahl ist sinnvoll, da der vollständige Name und Typ der untergeordneten Ressource den Namen und den Typ der übergeordneten Ressource enthalten. Aus diesem Grund weist der vollständige Name weiterhin ein Segment weniger auf als der vollständige Typ.

       "resources": [
           {
               "type": "Microsoft.KeyVault/vaults",
               "name": "contosokeyvault",
               ...
               "resources": [
                   {
                       "type": "secrets",
                       "name": "appPassword",
                       ...
                   }
               ]
           }
       ]

   Die richtige Angabe der Segmente kann bei Resource Manager-Typen problematisch sein, die ressourcenanbieterübergreifend angewendet werden. Wenn Sie beispielsweise eine Website mit einer Ressourcensperre belegen möchten, benötigen Sie einen Typ mit vier Segmenten. Folglich umfasst der Name drei Segmente:

       {
           "type": "Microsoft.Web/sites/providers/locks",
           "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
           ...
       }

- copy-Index wird nicht erwartet

   Dieser **InvalidTemplate**-Fehler tritt auf, wenn Sie das **copy**-Element auf einen Teil der Vorlage angewendet haben, der dieses Element nicht unterstützt. Sie können das copy-Element nur auf einen Ressourcentyp anwenden. Sie können „copy“ nicht auf eine Eigenschaft in einem Ressourcentyp anwenden. Beispiel: Sie wenden „copy“ auf einen virtuellen Computer an, können das Element aber nicht auf die Betriebssystem-Datenträger für einen virtuellen Computer anwenden. In einigen Fällen können Sie eine untergeordnete Ressource in eine übergeordnete Ressource umwandeln, um eine Kopierschleife zu erstellen. Weitere Informationen zur Verwendung von „copy“ finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager](resource-group-create-multiple.md).

- Parameter ist ungültig

   Wenn die Vorlage zulässige Werte für einen Parameter angibt und Sie einen Wert bereitstellen, der keinem dieser Werte entspricht, erhalten Sie eine Fehlermeldung ähnlich der folgenden:

       Code=InvalidTemplate;
       Message=Deployment template validation failed: 'The provided value {parameter value}
       for the template parameter {parameter name} is not valid. The parameter value is not
       part of the allowed values

   Überprüfen Sie die zulässigen Werte in der Vorlage, und geben Sie während der Bereitstellung einen gültigen Wert an.

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a>NotFound und ResourceNotFound
Wenn Ihre Vorlage einen Ressourcennamen enthält, der nicht aufgelöst werden kann, erhalten Sie eine Fehlermeldung ähnlich der folgenden:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Wenn Sie versuchen, die fehlende Ressource in der Vorlage bereitzustellen, prüfen Sie, ob Sie eine Abhängigkeit hinzufügen müssen. Resource Manager optimiert die Bereitstellung, indem, sofern möglich, gleichzeitig Ressourcen erstellt werden. Wenn eine Ressource nach einer anderen Ressource bereitgestellt werden muss, müssen Sie das **dependsOn**-Element in der Vorlage verwenden, um eine Abhängigkeit zu der anderen Ressource herzustellen. Beim Bereitstellen einer Web-App muss z. B. der App Service-Plan vorhanden sein. Wenn Sie nicht angegeben haben, dass die Web-App vom App Service-Plan abhängig ist, erstellt Resource Manager beide Ressourcen zur gleichen Zeit. Beim Versuch, eine Eigenschaft für die Web-App festzulegen, erhalten Sie eine Fehlermeldung, dass die App Service-Planressource nicht gefunden werden kann, da sie noch nicht vorhanden ist. Sie verhindern diesen Fehler, indem Sie die Abhängigkeit in der Web-App festlegen.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }

Dieser Fehler wird auch angezeigt, wenn die Ressource in einer anderen Ressourcengruppe als der vorhanden ist, in der die Ressource bereitgestellt wird. Verwenden Sie in diesem Fall die [resourceId-Funktion](resource-group-template-functions.md#resourceid), um den vollqualifizierten Namen der Ressource abzurufen.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    }

Wenn Sie versuchen, die Funktionen [reference](resource-group-template-functions.md#reference) oder [listKeys](resource-group-template-functions.md#listkeys) mit einer Ressource zu verwenden, die nicht aufgelöst werden kann, erhalten Sie folgenden Fehler:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
    group {resource group name} was not found.

Suchen Sie nach einem Ausdruck, der die **reference**-Funktion enthält. Überprüfen Sie, ob die Parameterwerte richtig sind.

### <a name="parentresourcenotfound"></a>ParentResourceNotFound

Wenn eine Ressource einer anderen übergeordnet ist, muss die übergeordnete Ressource vor dem Erstellen der untergeordneten Ressource bereits vorhanden sein. Wenn diese noch nicht vorhanden ist, erhalten Sie den folgenden Fehler:

     Code=ParentResourceNotFound;
     Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."

Der Name der untergeordneten Ressource enthält den Namen der übergeordneten Ressource. Zum Beispiel könnte eine SQL-Datenbank wie folgt definiert werden:

    {
      "type": "Microsoft.Sql/servers/databases",
      "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
      ...

Wenn Sie allerdings keine Abhängigkeit von der übergeordneten Ressource angeben, wird die untergeordnete Ressource möglicherweise vor der übergeordneten bereitgestellt. Um diesen Fehler aufzulösen, geben Sie eine Abhängigkeit an.

    "dependsOn": [
        "[variables('databaseServerName')]"
    ]

<a id="storagenamenotunique" />
### <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a>StorageAccountAlreadyExists und StorageAccountAlreadyTaken
Bei Speicherkonten müssen Sie einen Namen für die Ressource angeben, der in Azure eindeutig ist. Wenn Sie keinen eindeutigen Namen angeben, erhalten Sie einen Fehler wie diesen:

    Code=StorageAccountAlreadyTaken
    Message=The storage account named mystorage is already taken.

Sie können einen eindeutigen Namen erstellen, indem Sie Ihre Benennungskonvention mit dem Ergebnis der [uniqueString](resource-group-template-functions.md#uniquestring) -Funktion verketten.

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Wenn Sie ein Speicherkonto bereitstellen, das den gleichen Namen hat wie ein in Ihrem Abonnement vorhandenes Speicherkonto, aber einen anderen Speicherort angeben, erhalten Sie eine Fehlermeldung, dass das Speicherkonto bereits an einem anderen Speicherort existiert. Löschen Sie das vorhandene Speicherkonto, oder geben Sie den gleichen Speicherort wie für das vorhandene Speicherkonto an.

### <a name="accountnameinvalid"></a>AccountNameInvalid
Beim Versuch, einem Speicherkonto einen Namen zuzuweisen, der nicht zulässige Zeichen enthält, wird Ihnen der Fehler **AccountNameInvalid** angezeigt. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.

### <a name="badrequest"></a>BadRequest

Sie erhalten möglicherweise den Status „BadRequest“, wenn Sie einen ungültigen Wert für die Eigenschaft angeben. Wenn Sie zum Beispiel einen falschen SKU-Wert für ein Speicherkonto angeben, tritt bei der Bereitstellung ein Fehler auf. 

<a id="noregisteredproviderfound" />
### <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a>„NoRegisteredProviderFound“ und „MissingSubscriptionRegistration“
Beim Bereitstellen von Ressourcen werden möglicherweise der folgende Fehlercode und die folgende Fehlermeldung angezeigt:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation}
    and API version {api-version} for type {resource-type}.

Oder Sie erhalten eine ähnliche Meldung, die Folgendes besagt:

    Code: MissingSubscriptionRegistration
    Message: The subscription is not registered to use namespace {resource-provider-namespace}

Für diese Fehler gibt es drei Gründe:

1. Der Ressourcenanbieter wurde für Ihr Abonnement nicht registriert.
2. Die API-Version wird für den Ressourcentyp nicht unterstützt.
3. Der Standort wird für den Ressourcentyp nicht unterstützt.

Die Fehlermeldung enthält in der Regel Vorschläge für die unterstützten Standorte und API-Versionen. Sie können Ihre Vorlage in einen der vorgeschlagenen Werte ändern. Die meisten Anbieter werden automatisch über das Azure-Portal oder die verwendete Befehlszeilenschnittstelle registriert. Wenn Sie zuvor noch keinen bestimmten Ressourcenanbieter verwendet haben, müssen Sie diesen Anbieter unter Umständen registrieren. Weitere Informationen zu Ressourcenanbietern erhalten Sie in PowerShell oder der Azure-Befehlszeilenschnittstelle.

**Portal**

Sie können den Registrierungsstatus sehen und einen Ressourcenanbieter-Namespace über das Portal registrieren.

1. Wählen Sie für Ihr Abonnement die Option **Ressourcenanbieter** aus.

   ![Ressourcenanbieter auswählen](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. Betrachten Sie die Liste der Ressourcenanbieter, und wählen Sie bei Bedarf den Link **Registrieren** aus, um den Ressourcenanbietertyp zu registrieren, den Sie bereitstellen möchten.

   ![Liste der Ressourcenanbieter](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

**PowerShell**

Verwenden Sie zum Anzeigen des Registrierungsstatus **Get-AzureRmResourceProvider**.

    Get-AzureRmResourceProvider -ListAvailable

Verwenden Sie **Register-AzureRmResourceProvider** , um einen Anbieter zu registrieren, und geben Sie den Namen des zu registrierenden Ressourcenanbieters an.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Verwenden Sie zum Abrufen der unterstützten Standorte für einen bestimmten Ressourcentyp Folgendes:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Verwenden Sie zum Abrufen der unterstützten API-Versionen für einen bestimmten Ressourcentyp Folgendes:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

**Azure-CLI**

Mit dem Befehl `azure provider list` können Sie ermitteln, ob der Anbieter registriert ist.

    azure provider list

Verwenden Sie den Befehl `azure provider register` , um einen Ressourcenanbieter zu registrieren, und geben Sie den zu registrierenden *Namespace* an.

    azure provider register Microsoft.Cdn

Verwenden Sie zum Anzeigen der unterstützten Standorte und API-Versionen für einen Ressourcenanbieter Folgendes:

    azure provider show -n Microsoft.Compute --json > compute.json

<a id="quotaexceeded" />
### <a name="quotaexceeded-and-operationnotallowed"></a>QuotaExceeded und OperationNotAllowed
Probleme können auftreten, wenn eine Bereitstellung ein Kontingent überschreitet (etwa für eine Ressourcengruppe, ein Abonnement, ein Konto oder Ähnliches). Ihr Abonnement kann beispielsweise so konfiguriert werden, um die Anzahl der Kerne für eine Region zu begrenzen. Wenn Sie versuchen, einen virtuellen Computer mit mehr als der zulässigen Anzahl von Kernen bereitzustellen, erhalten Sie eine Fehlermeldung, die darauf hinweist, dass das Kontingent überschritten wurde.
Die vollständigen Kontingentinformationen finden Sie unter [Grenzwerte, Kontingente und Einschränkungen für Azure-Abonnements und -Dienste](../azure-subscription-service-limits.md).

Mit dem Befehl `azure vm list-usage` können Sie über die Azure-Befehlszeilenschnittstelle die Kernkontingente Ihres eigenen Abonnements untersuchen. Im folgenden Beispiel wird veranschaulicht, dass das Kernkontingent für ein kostenloses Testkonto 4 ist:

    azure vm list-usage

Ausgabe des Befehls:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Wenn Sie in der Region „USA, Westen“ eine Vorlage bereitstellen, die mehr als vier Kerne erstellt, erhalten Sie einen Bereitstellungsfehler ähnlich dem folgenden:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core.
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

In PowerShell können Sie alternativ das Cmdlet **Get-AzureRmVMUsage** verwenden.

    Get-AzureRmVMUsage

Ausgabe des Befehls:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

In diesen Fällen sollten Sie zum Portal navigieren und ein Supportproblem einreichen, um Ihr Kontingent für die Region, in der Sie diese bereitstellen möchten, zu erhöhen.

> [!NOTE]
> Denken Sie daran, dass für Ressourcengruppen das Kontingent für jede einzelne Region und nicht für das gesamte Abonnement gilt. Wenn Sie 30 Kerne in der Region "USA, Westen" bereitstellen möchten, müssen Sie 30 Ressourcen-Manager-Kerne für "USA, Westen" anfordern. Wenn Sie 30 Kerne in einer der Regionen bereitstellen möchten, auf die Sie Zugriff haben, sollten Sie 30 Resource Manager-Kerne in allen Regionen anfordern.
>
>

### <a name="invalidcontentlink"></a>InvalidContentLink
Wenn Sie die folgende Fehlermeldung erhalten, gilt:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

Sie haben wahrscheinlich versucht, eine geschachtelte Vorlage zu verknüpfen, die nicht verfügbar ist. Überprüfen Sie den URI, den Sie für die geschachtelte Vorlage angegeben haben. Wenn die Vorlage in einem Speicherkonto vorhanden ist, stellen Sie sicher, dass auf den URI zugegriffen werden kann. Möglicherweise müssen Sie ein SAS-Token übergeben. Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure-Ressourcen-Manager](resource-group-linked-templates.md).

### <a name="requestdisallowedbypolicy"></a>RequestDisallowedByPolicy
Sie erhalten diesen Fehler, wenn Ihr Abonnement eine Ressourcenrichtlinie enthält, die eine Aktion verhindert, die Sie während der Bereitstellung ausführen möchten. Suchen Sie in der Fehlermeldung die Richtlinienkennung.

    Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'

Geben Sie in **PowerShell** diese Richtlinienkennung als **Id**-Parameter an, um Details zur Richtlinie abzurufen, die Ihre Bereitstellung blockiert.

    (Get-AzureRmPolicyAssignment -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json

Geben Sie an der **Azure-Befehlszeilenschnittstelle** den Namen der Richtliniendefinition an:

    azure policy definition show regionPolicyDefinition --json

Weitere Informationen zu Richtlinien finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).

### <a name="authorization-failed"></a>Fehler bei der Autorisierung
Möglicherweise wird während der Bereitstellung ein Fehler angezeigt, da das Konto oder ein Dienstprinzipal, der versucht die Ressourcen bereitzustellen, keinen Zugriff zum Ausführen dieser Aktionen hat. Mit Azure Active Directory können Sie oder Ihr Administrator sehr genau kontrollieren, welche Identitäten auf welche Ressourcen Zugriff haben. Wenn Ihrem Konto die Leserrolle zugewiesen ist, können Sie keine neuen Ressourcen erstellen. In diesem Fall wird eine Fehlermeldung angezeigt, die darauf hinweist, dass die Autorisierung fehlgeschlagen ist.

Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure Active Directory-Ressourcen](../active-directory/role-based-access-control-configure.md).

### <a name="skunotavailable"></a>SkuNotAvailable

Beim Bereitstellen einer Ressource (in der Regel ein virtueller Computer) werden möglicherweise der folgende Fehlercode und die folgende Fehlermeldung angezeigt:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

Sie erhalten diesen Fehler, wenn die ausgewählte Ressourcen-SKU (z.B. die Größe des virtuellen Computers) für den ausgewählten Standort nicht verfügbar ist. Sie haben zwei Optionen, dieses Problem zu beheben:

- Melden Sie sich beim Portal an, und fügen Sie über die Benutzeroberfläche eine neue Ressource hinzu. Beim Festlegen der Werte werden die verfügbaren SKUs für die Ressource angezeigt. Sie müssen die Bereitstellung nicht abschließen.

    ![Verfügbare SKUs](./media/resource-manager-common-deployment-errors/view-sku.png)

- Wenn Sie keine geeignete SKU in dieser oder einer anderen Region finden, die Ihre Geschäftsanforderungen erfüllt, wenden Sie sich an den [Azure-Support](https://portal.azure.com/#create/Microsoft.Support).

## <a name="troubleshooting-tricks-and-tips"></a>Tipps und Tricks für die Problembehandlung

### <a name="enable-debug-logging"></a>Debugprotokollierung aktivieren
Sie können wertvolle Informationen darüber sammeln, wie Ihre Bereitstellung verarbeitet wird, indem Sie die Anforderung, die Antwort oder beides protokollieren.

- PowerShell

   Legen Sie in PowerShell den Parameter **DeploymentDebugLogLevel** auf „All“, „ResponseContent“ oder „RequestContent“ fest.

       New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All

   Überprüfen Sie den Anforderungsinhalt mit folgendem Cmdlet:

       (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json

   Oder überprüfen Sie den Antwortinhalt mit:

       (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json

   Mithilfe dieser Informationen können Sie ermitteln, ob ein Wert in der Vorlage nicht ordnungsgemäß festgelegt wurde.

- Azure-Befehlszeilenschnittstelle

   Legen Sie in der Azure-Befehlszeilenschnittstelle den Parameter **--debug-setting** auf „All“, „ResponseContent“ oder „RequestContent“ fest.

       azure group deployment create --debug-setting All -f c:\Azure\Templates\storage.json -g examplegroup -n ExampleDeployment

   Überprüfen Sie den Inhalt der protokollierten Anforderung und Antwort durch folgenden Befehl:

       azure group deployment operation list --resource-group examplegroup --name ExampleDeployment --json

   Mithilfe dieser Informationen können Sie ermitteln, ob ein Wert in der Vorlage nicht ordnungsgemäß festgelegt wurde.

- Geschachtelte Vorlage

   Verwenden Sie zum Protokollieren von Debuginformationen zu einer geschachtelten Vorlage das **debugSetting**-Element.

       {
           "apiVersion": "2016-09-01",
           "name": "nestedTemplate",
           "type": "Microsoft.Resources/deployments",
           "properties": {
               "mode": "Incremental",
               "templateLink": {
                   "uri": "{template-uri}",
                   "contentVersion": "1.0.0.0"
               },
               "debugSetting": {
                  "detailLevel": "requestContent, responseContent"
               }
           }
       }


### <a name="create-a-troubleshooting-template"></a>Erstellen einer Vorlage zur Problembehandlung
Mitunter ist die einfachste Möglichkeit für die Behandlung von Problemen bei Ihrer Vorlage das Testen von Teilen davon. Sie können eine vereinfachte Vorlage erstellen, die es Ihnen ermöglicht, sich auf den Teil zu konzentrieren, der Ihrer Meinung nach den Fehler verursacht. Nehmen wir beispielsweise an, dass ein Fehler auftritt, wenn Sie auf eine Ressource verweisen. Anstatt sich mit einer gesamten Vorlage zu beschäftigen, erstellen Sie eine Vorlage, die den Teil wiedergibt, der Ihr Problem möglicherweise verursacht. So können Sie besser ermitteln, ob die richtigen Parameter übergeben, Vorlagenfunktionen ordnungsgemäß genutzt und die Ressourcen abgerufen werden, die Sie erwarten.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageName": {
            "type": "string"
        },
        "storageResourceGroup": {
            "type": "string"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "exampleOutput": {
            "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
            "type" : "object"
        }
      }
    }

Ein anderes Beispiel: Es treten Bereitstellungsfehler auf, von denen Sie annehmen, dass sie aufgrund falsch festgelegter Abhängigkeiten entstehen. Testen Sie Ihre Vorlage, indem Sie sie in einfachere Vorlagen aufteilen. Erstellen Sie zunächst eine Vorlage, mit der nur eine einzige Ressource bereitgestellt wird (z.B. eine SQL Server-Instanz). Wenn Sie sicher sind, dass die Ressource richtig definiert ist, fügen Sie eine Ressource hinzu, die davon abhängig ist (z.B. eine SQL-Datenbank). Wenn diese beiden Ressourcen richtig definiert sind, fügen Sie weitere abhängige Ressourcen hinzu (z.B. Überwachungsrichtlinien). Löschen Sie zwischen den jeweiligen Testbereitstellungen die Ressourcengruppe, um sicherzustellen, dass Sie die Abhängigkeiten angemessen testen. 

### <a name="check-deployment-sequence"></a>Überprüfen der Bereitstellungssequenz

Viele Bereitstellungsfehler treten auf, wenn Ressourcen in einer unerwarteten Reihenfolge bereitgestellt werden. Diese Fehler treten auf, wenn Abhängigkeiten nicht ordnungsgemäß festgelegt sind. Eine Ressource versucht, einen Wert für eine andere Ressource zu verwenden, die andere Ressource ist jedoch noch nicht vorhanden. So zeigen Sie die Reihenfolge der Vorgänge bei der Bereitstellung an:

1. Wählen Sie den Bereitstellungsverlauf für die Ressourcengruppe aus.

   ![Bereitstellungsverlauf auswählen](./media/resource-manager-common-deployment-errors/select-deployment.png)

2. Wählen Sie aus dem Verlauf eine Bereitstellung und **Ereignisse** aus.

   ![Bereitstellungsereignisse auswählen](./media/resource-manager-common-deployment-errors/select-deployment-events.png)

3. Überprüfen Sie die Abfolge von Ereignissen für jede Ressource. Achten Sie auf den Status der einzelnen Vorgänge. Die folgende Abbildung zeigt beispielsweise drei parallel bereitgestellte Speicherkonten. Beachten Sie, dass die drei Speicherkonten zur selben Zeit gestartet werden.

   ![Parallele Bereitstellung](./media/resource-manager-common-deployment-errors/deployment-events-parallel.png)

   Die nächste Abbildung zeigt drei Speicherkonten, die nicht parallel bereitgestellt werden. Das zweite Speicherkonto ist so markiert, dass es vom ersten Speicherkonto abhängig ist, und das dritte Speicherkonto ist abhängig vom zweiten Speicherkonto. Aus diesem Grund wird das erste Speicherkonto gestartet, akzeptiert und abgeschlossen, bevor das nächste gestartet wird.

   ![Sequenzielle Bereitstellung](./media/resource-manager-common-deployment-errors/deployment-events-sequence.png)

Sehen Sie sich die Bereitstellungsereignisse an, um festzustellen, ob eine Ressource früher als erwartet gestartet wurde. Wenn dies der Fall ist, überprüfen Sie die Abhängigkeiten für diese Ressource.

## <a name="troubleshooting-other-services"></a>Problembehandlung bei anderen Diensten
Wenn die oben genannten Fehlercodes der Bereitstellung bei der Behebung Ihres Problems nicht weiterhelfen, stehen für die verschiedenen Azure-Dienste ausführlichere Anleitungen zur Problembehandlung für die einzelnen Fehler zur Verfügung.

In der folgenden Tabelle sind die Themen für die Problembehandlung für virtuelle Computer aufgelistet.

| Fehler | Artikel |
| --- | --- |
| Fehler der benutzerdefinierten Skripterweiterung |[Fehler bei Erweiterungen für virtuelle Windows-Computer](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)<br />oder<br />[Problembehandlung für Fehler bei Azure-Erweiterungen für virtuelle Linux-Computer](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Fehler bei der Bereitstellung des Betriebssystemimage |[Fehler bei neuen virtuellen Windows-Computern](../virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)<br />oder<br />[Fehler bei neuen virtuellen Linux-Computern](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Fehler bei der Zuordnung |[Zuordnungsfehler bei virtuellen Windows-Computern](../virtual-machines/virtual-machines-windows-allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)<br />oder<br />[Problembehandlung für Zuordnungsfehler beim Erstellen, Neustarten oder Ändern der Größe von virtuellen Linux-Computern in Azure](../virtual-machines/virtual-machines-linux-allocation-failure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Secure Shell (SSH)-Fehler beim Herstellen einer Verbindung |[Problembehandlung für SSH-Verbindungen (Secure Shell) mit einem Linux-basierten virtuellen Azure-Computer](../virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Fehler beim Herstellen einer Verbindung mit einer Anwendung, die auf einem virtuellen Computer ausgeführt wird |[Auf einem virtuellen Windows-Computer ausgeführte Anwendung](../virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)<br />oder<br />[Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird (Linux)](../virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Fehler bei Remotedesktopverbindungen |[Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](../virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Verbindungsfehler, die durch eine erneute Bereitstellung behoben werden |[Einen virtuellen Computer in einem neuen Azure-Knoten erneut bereitstellen](../virtual-machines/virtual-machines-windows-redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Clouddienstfehler |[Behandeln von Problemen mit der Clouddienstbereitstellung](../cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

In der folgenden Tabelle sind die Themen für die Problembehandlung für andere Azure-Dienste aufgelistet. Der Schwerpunkt liegt hier auf Problemen im Zusammenhang mit der Bereitstellung oder Konfiguration von Ressourcen. Wenn Sie Hilfe bei der Problembehandlung von Laufzeitproblemen mit einer Ressource benötigen, lesen Sie die Informationen in der Dokumentation für den jeweiligen Azure-Dienst.

| Dienst | Artikel |
| --- | --- |
| Automation |[Tipps zur Problembehandlung für häufige Fehler in Azure Automation](../automation/automation-troubleshooting-automation-errors.md) |
| Azure Stack |[Microsoft Azure Stack troubleshooting (Problembehandlung für Microsoft Azure Stack)](../azure-stack/azure-stack-troubleshooting.md) |
| Data Factory |[Problembehandlung bei Data Factory](../data-factory/data-factory-troubleshoot.md) |
| Service Fabric |[Häufig auftretende Probleme und Problembehandlung beim Bereitstellen von Diensten in Azure Service Fabric](../service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Site Recovery |[Überwachung und Problembehandlung für den Schutz von virtuellen Computern und physischen Servern](../site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Speicher |[Microsoft Azure-Speicher: Überwachung, Diagnose und Problembehandlung](../storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple |[Beheben von Problemen mit der Bereitstellung von StorSimple-Geräten](../storsimple/storsimple-troubleshoot-deployment.md) |
| SQL-Datenbank |[Beheben von Verbindungsproblemen mit der Azure SQL-Datenbank](../sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| SQL Data Warehouse |[Problembehandlung bei Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="next-steps"></a>Nächste Schritte
* Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](resource-group-audit.md).
* Weitere Informationen zu Aktionen zum Bestimmen von Fehlern während der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen mit dem Azure-Portal](resource-manager-troubleshoot-deployments-portal.md).



<!--HONumber=Dec16_HO3-->


