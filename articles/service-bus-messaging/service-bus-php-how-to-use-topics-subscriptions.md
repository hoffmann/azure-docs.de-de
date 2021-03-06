---
title: Verwenden von Service Bus-Themen mit PHP | Microsoft Docs
description: "Erfahren Sie mehr über die Verwendung von Service Bus-Themen mit PHP in Azure."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: faaa4bbd-f6ef-42ff-aca7-fc4353976449
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 10/14/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c1d58e912f3b08c0eeff3724658301e8c6b28be5


---
# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Verwenden von Service Bus-Themen und -Abonnements
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In diesem Artikel erfahren Sie, wie Sie Service Bus-Themen und -Abonnements verwenden. Die Beispiele wurden in PHP geschrieben und verwenden das [Azure-SDK für PHP](../php-download-sdk.md). Die behandelten Szenarios umfassen **das Erstellen von Themen und Abonnements**, **das Erstellen von Abonnementfiltern**, **das Senden von Nachrichten an ein Thema**, **das Empfangen von Nachrichten von einem Abonnement** und **das Löschen von Themen und Abonnements**.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Erstellen einer PHP-Anwendung
Die einzige Voraussetzung für das Erstellen einer PHP-Anwendung, die auf den Azure-Blob-Dienst zugreift, ist das Verweisen auf Klassen im [Azure SDK für PHP](../php-download-sdk.md) aus dem Code heraus. Sie können die Anwendung mit beliebigen Entwicklungstools oder mit Editor erstellen.

> [!NOTE]
> In Ihrer PHP-Installation muss außerdem die [OpenSSL-Erweiterung](http://php.net/openssl) installiert und aktiviert sein.
> 
> 

In diesem Artikel wird beschrieben, wie Sie die Dienstfunktionen verwenden, die sowohl lokal in einer PHP-Anwendung aufgerufen werden können als auch als Code in einer Azure-Webrolle, -Workerrolle oder als Website

## <a name="get-the-azure-client-libraries"></a>Abrufen der Azure-Clientbibliotheken
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Ihrer Anwendung für die Verwendung von Service Bus
So verwenden Sie die Service Bus-APIs

1. Verweisen Sie mithilfe der [require_once][require-once]-Anweisung auf die Autoloaderdatei.
2. Verweisen Sie auf alle Klassen, die Sie möglicherweise verwenden.

Das folgende Beispiel zeigt, wie die Autoloaderdatei integriert und auf die **ServiceBusService**-Klasse verwiesen wird.

> [!NOTE]
> In diesem Beispiel (und in anderen Beispielen in diesem Artikel) wird angenommen, dass Sie die PHP-Clientbibliotheken für Azure über Composer installiert haben. Wenn Sie die Bibliotheken manuell oder als PEAR-Paket installiert haben, müssen Sie auf die Autoloaderdatei **WindowsAzure.php** verweisen.
> 
> 

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In den folgenden Beispielen wird die `require_once`-Anweisung immer angezeigt. Jedoch wird nur auf die für die Ausführung des Beispiels erforderlichen Klassen verwiesen.

## <a name="set-up-a-service-bus-connection"></a>Einrichten einer Service Bus-Verbindung
Um einen Service Bus-Client zu instanziieren, benötigen Sie zunächst eine gültige Verbindungszeichenfolge mit diesem Format:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Hierbei hat der `Endpoint` normalerweise das Format `https://[yourNamespace].servicebus.windows.net`.

Zum Erstellen eines Azure-Dienstclients müssen Sie die **ServicesBuilder**-Klasse verwenden. Sie können:

* die Verbindungszeichenfolge direkt an die Klasse weitergeben.
* den **CloudConfigurationManager (CCM)** verwenden, um mehrere externe Quellen für die Verbindungszeichenfolge zu überprüfen:
  * Standardmäßig wird eine externe Quelle unterstützt – Umgebungsvariablen.
  * Sie können neue Quellen durch Erweitern der **ConnectionStringSource** -Klasse hinzufügen.

Für die hier erläuterten Beispiele wird die Verbindungszeichenfolge direkt weitergegeben.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Erstellen eines Themas
Sie können Verwaltungsvorgänge für Service Bus-Themen über die **ServiceBusRestProxy**-Klasse durchführen. Ein **ServiceBusRestProxy**-Objekt wird über die **ServicesBuilder::createServiceBusService**-Factorymethode mit einer entsprechenden Verbindungszeichenfolge erstellt, die die Token-Berechtigungen für deren Verwaltung kapselt.

Das folgende Beispiel zeigt, wie Sie **ServiceBusRestProxy** instanziieren und **ServiceBusRestProxy->createTopic** aufrufen, um das Thema `mytopic` im Namespace `MySBNamespace` zu erstellen:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Sie können die `listTopics`-Methode bei `ServiceBusRestProxy`-Objekten verwenden, um zu überprüfen, ob ein Thema mit einem bestimmten Namen bereits innerhalb eines Dienstnamespace vorhanden ist.
> 
> 

## <a name="create-a-subscription"></a>Erstellen eines Abonnements
Themenabonnements werden ebenfalls mit der **ServiceBusRestProxy->createSubscription**-Methode erstellt. Abonnements werden benannt und können einen optionalen Filter aufweisen, der die Nachrichten einschränkt, die an die virtuelle Warteschlange des Abonnements übergeben werden.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit dem Standardfilter (MatchAll)
**MatchAll** ist der Standardfilter, der verwendet wird, wenn beim Erstellen eines neuen Abonnements kein Filter angegeben wird. Wenn der Filter **MatchAll** verwendet wird, werden alle für das Thema veröffentlichten Nachrichten in die virtuelle Warteschlange des Abonnements gestellt. Mit dem folgenden Beispiel wird ein Abonnement namens „mysubscription“ erstellt, für das der Standardfilter **MatchAll** verwendet wird.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filtern
Sie können auch Filter einrichten, durch die Sie angeben können, welche an ein Thema gesendeten Nachrichten in einem bestimmten Themenabonnement angezeigt werden sollen. Der von Abonnements unterstützte flexibelste Filtertyp ist **SqlFilter**, der eine Teilmenge von SQL92 implementiert. SQL-Filter werden auf die Eigenschaften der Nachrichten angewendet, die für das Thema veröffentlicht werden. Weitere Informationen über SqlFilters finden Sie unter [SqlFilter.SqlExpression-Eigenschaft][sqlfilter].

> [!NOTE]
> Jede Regel in einem Abonnement verarbeitet eingehende Nachrichten unabhängig und fügt ihre Ergebnisnachrichten dem Abonnement hinzu. Zusätzlich ist für jedes neue Abonnement ein Standard-**Rule**-Objekt mit einem Filter vorhanden, der dem Abonnement alle Nachrichten aus einem Thema hinzufügt. Wenn Sie nur Nachrichten erhalten wollen, die auf Ihren Filter passen, müssen Sie die Standardregel entfernen. Sie können die Standardregel entfernen, indem Sie die `ServiceBusRestProxy->deleteRule`-Methode verwenden.
> 
> 

Mit dem folgenden Beispiel wird das Abonnement **HighMessages** mit einem **SqlFilter**-Element erstellt, das nur Nachrichten auswählt, deren benutzerdefinierte **MessageNumber**-Eigenschaft größer als 3 ist (Informationen zum Hinzufügen benutzerdefinierter Eigenschaften zu Nachrichten finden Sie unter [Senden von Nachrichten an ein Thema](#send-messages-to-a-topic)):

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Beachten Sie, dass für diesen Code ein zusätzlicher Namespace erforderlich ist: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Im folgenden Beispiel wird in ähnlicher Weise ein Abonnement namens **LowMessages** mit einem **SqlFilter**-Objekt erstellt, bei dem nur Nachrichten ausgewählt werden, deren benutzerdefinierte **MessageNumber**-Eigenschaft kleiner oder gleich 3 ist:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Wenn jetzt eine Nachricht an das `mytopic`-Thema gesendet wird, wird diese nun stets an die Empfänger des `mysubscription`-Abonnements zugestellt. Sie wird selektiv an die Empfänger der Abonnements `HighMessages` und `LowMessages` zugestellt (je nach Inhalt der Nachricht).

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten an ein Thema
Um eine Nachricht an ein Service Bus-Thema zu senden, ruft Ihre Anwendung die **ServiceBusRestProxy->sendTopicMessage**-Methode auf. Der folgende Code zeigt, wie Sie eine Nachricht an das zuvor erstellte `mytopic`-Thema im `MySBNamespace`-Dienstnamespace senden können.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

An Service Bus-Themen gesendete Nachrichten sind Instanzen der **BrokeredMessage**-Klasse. **BrokeredMessage**-Objekte enthalten eine Reihe von Standardeigenschaften und -methoden (wie etwa **getLabel**, **getTimeToLive**, **setLabel** und **setTimeToLive**) sowie Eigenschaften für die Aufnahme benutzerdefinierter anwendungsspezifischer Eigenschaften. Das folgende Beispiel zeigt das Senden von fünf Testnachrichten an das zuvor erstellte `mytopic`-Thema. Mit der **setProperty**-Methode wird jeder Nachricht eine benutzerdefinierte Eigenschaft (`MessageNumber`) hinzugefügt. Beachten Sie, wie der Wert der `MessageNumber`-Eigenschaft in jeder Nachricht variiert (auf diese Weise können Sie bestimmen, welche Abonnements sie erhalten, wie im Abschnitt [Erstellen eines Abonnements](#create-a-subscription) erläutert):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);

    // Set custom property.
    $message->setProperty("MessageNumber", $i);

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße von 256 KB im [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB im [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten, die ein Thema enthält. Es gibt jedoch eine Obergrenze für die Gesamtgröße der Nachrichten eines Themas. Die Obergrenze für die Themengröße beträgt 5 GB. Weitere Informationen zu Kontingenten in Service Bus finden Sie unter [Service Bus-Kontingente][Service Bus-Kontingente].

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement
Der beste Weg zum Empfangen von Nachrichten aus einem Abonnement ist die **ServiceBusRestProxy->receiveSubscriptionMessage**-Methode. Empfangene Nachrichten können in zwei unterschiedlichen Modi verwendet werden: **ReceiveAndDelete** (Standard) und **PeekLock**.

Bei Verwendung des **ReceiveAndDelete**-Modus ist der Nachrichtenempfang ein Single-Shot-Vorgang. Dies bedeutet: Wenn Service Bus eine Leseanforderung für eine Nachricht in einem Abonnement erhält, wird die Nachricht als verarbeitet gekennzeichnet und an die Anwendung zurückgesendet. Der **ReceiveAndDelete**-Modus ist das einfachste Modell. Es wird am besten für Szenarios eingesetzt, bei denen es eine Anwendung tolerieren kann, wenn eine Nachricht bei Auftreten eines Fehlers nicht verarbeitet wird. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Da Service Bus die Nachricht als verwendet markiert hat, wird er jene Nachricht auslassen, die vor dem Absturz verwendet wurde, wenn die Anwendung neu startet und erneut mit der Verwendung von Nachrichten beginnt.

Im **PeekLock**-Modus ist der Nachrichtenempfang zweistufig. Dadurch können Anwendungen unterstützt werden, die das Auslassen bzw. Fehlen von Nachrichten nicht zulassen können. Wenn Service Bus eine Anfrage erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann zurück an die Anwendung. Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt sie die zweite Phase des Empfangsprozesses durch Aufrufen von **ServiceBusRestProxy->deleteMessage** durch. Wenn Service Bus den **deleteMessage**-Aufruf erkennt, markiert er die Nachricht als verwendet und entfernt sie aus der Warteschlange.

Das folgende Beispiel zeigt, wie eine Nachricht mit dem (nicht standardmäßig verwendeten) **PeekLock**-Modus empfangen und verarbeitet werden kann. 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandeln von Anwendungsabstürzen und nicht lesbaren Nachrichten
Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine empfangene Anwendung eine Nachricht aus einem beliebigen Grund nicht verarbeiten kann, kann sie die Methode **unlockMessage** für die empfangene Nachricht aufrufen (anstelle der Methode **deleteMessage**). Dies führt dazu, dass Service Bus die Nachricht innerhalb der Warteschlange entsperrt und verfügbar macht, damit sie erneut empfangen werden kann, und zwar entweder durch dieselbe verarbeitende Anwendung oder durch eine andere verarbeitende Anwendung.

Zudem wird einer in der Warteschlange gesperrten Anwendung ein Zeitlimit zugeordnet. Wenn die Anwendung die Nachricht vor Ablauf des Sperrzeitlimits nicht verarbeiten kann (zum Beispiel wenn die Anwendung abstürzt), entsperrt Service Bus die Nachricht automatisch und macht sie verfügbar, um erneut empfangen zu werden.

Falls die Anwendung nach der Verarbeitung der Nachricht, aber vor Ausgabe der **deleteMessage**-Anforderung abstürzt, wird die Nachricht wieder an die Anwendung zugestellt, wenn diese neu gestartet wird. Dies wird häufig als **At Least Once Processing**(Verarbeitung mindestens einmal) bezeichnet und bedeutet, dass jede Nachricht mindestens einmal verarbeitet wird, wobei dieselbe Nachricht in bestimmten Situationen möglicherweise erneut zugestellt wird. Wenn eine doppelte Verarbeitung im betreffenden Szenario nicht geeignet ist, sollten Anwendungsentwickler Anwendungen zusätzliche Logik für den Umgang mit der Übermittlung doppelter Nachrichten hinzufügen. Dies wird häufig durch die Verwendung der **getMessageId**-Methode der Nachricht erzielt, die über mehrere Zustellversuche hinweg konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements
Verwenden Sie zum Löschen eines Themas oder Abonnements die **ServiceBusRestProxy->deleteTopic**-Methode bzw. die **ServiceBusRestProxy->deleteSubscripton**-Methode. Beachten Sie, dass durch das Löschen eines Themas auch alle Abonnements gelöscht werden, die mit dem Thema registriert sind.

Im folgenden Beispiel wird gezeigt, wie ein Thema (`mytopic`) und die darin registrierten Abonnements gelöscht werden.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Wenn Sie die **deleteSubscription**-Methode verwenden, können Sie ein Abonnement unabhängig löschen:

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun mit den Grundlagen von Service Bus-Warteschlangen vertraut sind, finden Sie weitere Informationen unter [Service Bus-Warteschlangen, -Themen und -Abonnements][Service Bus-Warteschlangen, -Themen und -Abonnements].

[Service Bus-Warteschlangen, -Themen und -Abonnements]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Service Bus-Kontingente]: service-bus-quotas.md



<!--HONumber=Nov16_HO3-->


