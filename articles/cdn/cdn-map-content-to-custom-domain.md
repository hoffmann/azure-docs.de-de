---
title: "Zuordnen einer benutzerdefinierten Domäne zu einem Azure CDN-Endpunkt (Content Delivery Network, Netzwerk für die Inhaltsübermittlung) | Microsoft Docs"
description: "In diesem Thema wird veranschaulicht, wie Sie CDN-Inhalte einer benutzerdefinierten Domäne zuordnen."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 567e72805c22c8100ef2d8d97c1e26a77f214ca3


---
# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Zuordnen einer benutzerdefinierten Domäne zu einem CDN-Endpunkt (Content Delivery Network, Netzwerk für die Inhaltsübermittlung)
Sie können einem CDN-Endpunkt (Content Delivery Network, Netzwerk für die Inhaltsübermittlung) eine benutzerdefinierte Domäne zuordnen, damit in URLs für zwischengespeicherte Inhalte anstatt einer Unterdomäne von „azureedge.net“ Ihr eigener Domänenname verwendet wird.

Es gibt zwei Möglichkeiten, einem CDN-Endpunkt eine benutzerdefinierte Domäne zuzuordnen:

1. [Registrieren eines CNAME-Eintrags bei Ihrer Domänenregistrierungsstelle und Zuordnen Ihrer benutzerdefinierten Domäne und Unterdomäne zum CDN-Endpunkt](#register-a-custom-domain-for-an-azure-cdn-endpoint)
   
    Ein CNAME-Eintrag ist ein DNS-Feature, das eine Quelldomäne wie `www.contosocdn.com` oder `cdn.contoso.com` einer Zieldomäne zuordnet. In diesem Fall ist die Quelldomäne Ihre benutzerdefinierte Domäne und Unterdomäne (eine Unterdomäne wie **www** oder **cdn** ist immer erforderlich). Die Zieldomäne entspricht Ihrem CDN-Endpunkt.  
   
    Die Zuordnung der benutzerdefinierten Domäne zum CDN-Endpunkt kann jedoch zu einer kurzzeitigen Downtime für die Domäne führen, während Sie die Domäne im Azure-Portal registrieren.
2. [Hinzufügen eines zwischengeschalteten Registrierungsschritts mithilfe von **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)
   
    Wenn Ihre benutzerdefinierte Domäne zurzeit eine Anwendung unterstützt, die unter einer Vereinbarung zum Servicelevel (SLA, Service Level Agreement) ohne Ausfallzeit läuft, können Sie mithilfe der Azure-Unterdomäne **cdnverify** einen Zwischenschritt für die Registrierung bereitstellen, damit Benutzer während der DNS-Zuordnung auf die Domäne zugreifen können.  

Nachdem Sie Ihre benutzerdefinierte Domäne mit einem der oben genannten Verfahren registriert haben, sollten Sie [überprüfen, ob die benutzerdefinierte Unterdomäne auf Ihren CDN-Endpunkt verweist](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [!NOTE]
> Sie müssen einen CNAME-Eintrag bei der Domänenregistrierungsstelle registrieren, um die Domäne dem CDN-Endpunkt zuzuordnen. CNAME-Einträge ordnen bestimmte Unterdomänen wie `www.contoso.com` oder `cdn.contoso.com` zu. Es ist nicht möglich, einen CNAME-Eintrag einer Stammdomäne wie `contoso.com`zuzuordnen.
> 
> Sie müssen einem CDN-Endpunkt eine dedizierte Unterdomäne zuordnen. Der erstellte CNAME-Eintrag leitet sämtlichen Datenverkehr für die Unterdomäne an den angegebenen Endpunkt weiter.  Wenn Sie dem CDN-Endpunkt beispielsweise die Unterdomäne `www.contoso.com` zuordnen, können Sie diese Unterdomäne keinem anderen Microsoft Azure-Endpunkt, z.B. einem Speicherkonto- oder Clouddienst-Endpunkt, zuordnen. Innerhalb einer Domäne können Sie jedoch verschiedene Unterdomänen für unterschiedliche Dienstendpunkte verwenden. Außerdem haben Sie die Möglichkeit, demselben CDN-Endpunkt verschiedene Unterdomänen zuzuordnen.
> 
> Beachten Sie, dass es bei **Azure CDN von Verizon**-Endpunkten (Standard und Premium) bis zu **90 Minuten** dauern kann, Änderungen der benutzerdefinierten Domäne an die CDN-Edgeknoten weiterzugeben.
> 
> 

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Registrieren einer benutzerdefinierten Domäne für einen Azure CDN-Endpunkt
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie auf **Durchsuchen**, auf **CDN-Profile** und anschließend auf das CDN-Profil mit dem Endpunkt, das Sie einer benutzerdefinierten Domäne zuordnen möchten.  
3. Klicken Sie auf dem Blatt **CDN-Profil** auf den CDN-Endpunkt, den Sie der Unterdomäne zuordnen möchten.
4. Klicken Sie am oberen Rand des Blatts für den Endpunkt auf die Schaltfläche **Benutzerdefinierte Domäne hinzufügen** .  Auf dem Blatt **Benutzerdefinierte Domäne hinzufügen** sehen Sie den vom CDN-Endpunkt abgeleiteten Hostnamen, den Sie zum Erstellen eines neuen CNAME-Eintrags verwenden können. Die Hostnamensadresse wird im Format **&lt;EndpointName>.azureedge.net** angezeigt.  Sie können diesen Hostnamen kopieren, um ihn für den CNAME-Eintrag zu verwenden.  
5. Navigieren Sie zur Website der Domänenregistrierungsstelle, und suchen Sie den Abschnitt für die Erstellung von DNS-Einträgen. Diese finden Sie beispielsweise in einem Abschnitt mit der Bezeichnung **Domänenname**, **DNS** oder **Namenserververwaltung**.
6. Suchen Sie den Abschnitt zur Verwaltung von CNAMEs. Öffnen Sie ggf. die Seite mit erweiterten Einstellungen, und suchen Sie nach den Wörtern CNAME, Alias oder Unterdomänen.
7. Erstellen Sie einen neuen CNAME-Eintrag, der die ausgewählte Unterdomäne (z.B. **www** oder **cdn**) dem im Blatt **Benutzerdefinierte Domänen hinzufügen** bereitgestellten Hostnamen zuordnet.
8. Kehren Sie zum Blatt **Benutzerdefinierte Domänen hinzufügen** zurück, und geben Sie den Namen der benutzerdefinierten Domäne einschließlich Unterdomäne in das Dialogfeld ein. Geben Sie z.B. den Domänennamen im Format `www.contoso.com` oder `cdn.contoso.com` ein.   
   
   Azure überprüft, ob der CNAME-Eintrag für den eingegebenen Domänennamen vorhanden ist. Wenn der CNAME-Eintrag ordnungsgemäß ist, wird Ihre benutzerdefinierte Domäne überprüft.  Bei **Azure CDN von Verizon** -Endpunkten (Standard und Premium) kann es bis zu 90 Minuten dauern, Einstellungen der benutzerdefinierten Domäne an alle CDN-Edgeknoten weiterzugeben.  
   
   Es kann eine Weile dauern, bis der CNAME-Eintrag an alle Namensserver im Internet weitergegeben wird. Wenn die Domäne nicht sofort erkannt wird, Sie aber sicher sind, dass der CNAME-Eintrag richtig ist, warten Sie einige Minuten, und überprüfen Sie die Domäne dann erneut.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Registrieren einer benutzerdefinierten Domäne für einen Azure CDN-Endpunkt mithilfe der übergangsweise verwendeten Unterdomäne "cdnverify"
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie auf **Durchsuchen**, auf **CDN-Profile** und anschließend auf das CDN-Profil mit dem Endpunkt, das Sie einer benutzerdefinierten Domäne zuordnen möchten.  
3. Klicken Sie auf dem Blatt **CDN-Profil** auf den CDN-Endpunkt, den Sie der Unterdomäne zuordnen möchten.
4. Klicken Sie am oberen Rand des Blatts für den Endpunkt auf die Schaltfläche **Benutzerdefinierte Domäne hinzufügen** .  Auf dem Blatt **Benutzerdefinierte Domäne hinzufügen** sehen Sie den vom CDN-Endpunkt abgeleiteten Hostnamen, den Sie zum Erstellen eines neuen CNAME-Eintrags verwenden können. Die Hostnamensadresse wird im Format **&lt;EndpointName>.azureedge.net** angezeigt.  Sie können diesen Hostnamen kopieren, um ihn für den CNAME-Eintrag zu verwenden.
5. Navigieren Sie zur Website der Domänenregistrierungsstelle, und suchen Sie den Abschnitt für die Erstellung von DNS-Einträgen. Diese finden Sie beispielsweise in einem Abschnitt mit der Bezeichnung **Domänenname**, **DNS** oder **Namenserververwaltung**.
6. Suchen Sie den Abschnitt zur Verwaltung von CNAMEs. Möglicherweise müssen Sie eine Seite mit erweiterten Einstellungen aufrufen und nach den Wörtern **CNAME**, **Alias** oder **Unterdomänen** suchen.
7. Erstellen Sie einen neuen CNAME-Eintrag, und geben Sie einen Unterdomänenalias ein, der die Unterdomäne **cdnverify** enthält. Die angegebene Unterdomäne kann beispielsweise das Format **cdnverify.www** oder **cdnverify.cdn** aufweisen. Geben Sie anschließend den Hostnamen an (dies ist Ihr CDN-Endpunkt). Verwenden Sie dabei das Format **cdnverify.&lt;Endpunktname>.azureedge.net**.
8. Kehren Sie zum Blatt **Benutzerdefinierte Domänen hinzufügen** zurück, und geben Sie den Namen der benutzerdefinierten Domäne einschließlich Unterdomäne in das Dialogfeld ein. Geben Sie z.B. den Domänennamen im Format `www.contoso.com` oder `cdn.contoso.com` ein. Beachten Sie, dass es in diesem Schritt nicht erforderlich ist, der Unterdomäne **decdnverify** voranzustellen.  
   
    Azure überprüft, ob der CNAME-Eintrag für den eingegebenen Domänennamen "cdnverify" vorhanden ist.
9. Die benutzerdefinierte Domäne wurde jetzt zwar von Azure überprüft, aber der an die Domäne gerichtete Datenverkehr wird noch nicht an Ihren CDN-Endpunkt weitergeleitet. Nachdem Sie lange genug für das Weitergeben der benutzerdefinierten Domäneneinstellungen an die Edgeknoten des CDN gewartet haben (90 Minuten für **Azure CDN von Verizon**, 1-2 Minuten für **Azure CDN von Akamai**), kehren Sie zur Website Ihrer DNS-Registrierungsstelle zurück, und erstellen Sie einen weiteren CNAME-Eintrag, der Ihre Unterdomäne Ihrem CDN-Endpunkt zuordnet. Geben Sie die Unterdomäne beispielsweise als **www** oder **cdn** und den Hostnamen als **&lt;Endpunktname>.azureedge.net** an. Mit diesem Schritt ist die Registrierung Ihrer benutzerdefinierten Domäne abgeschlossen.
10. Zuletzt können Sie den mithilfe von **cdnverify**erstellten CNAME-Eintrag löschen, da er nur als Übergangslösung benötigt wurde.  

## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>überprüfen, ob die benutzerdefinierte Unterdomäne auf Ihren CDN-Endpunkt verweist
* Nachdem Sie die Registrierung der benutzerdefinierten Domäne abgeschlossen haben, können Sie über die benutzerdefinierte Domäne auf den im CDN-Endpunkt zwischengespeicherten Inhalt zugreifen.
  Stellen Sie zunächst sicher, dass öffentliche Inhalte zum Zwischenspeichern auf dem Endpunkt verfügbar sind. Wenn der CDN-Endpunkt beispielsweise einem Speicherkonto zugeordnet ist, wird der Inhalt vom CDN in öffentlichen Blobcontainern zwischengespeichert. Zum Testen der benutzerdefinierten Domäne müssen Sie sicherstellen, dass der Container den öffentlichen Zugriff zulässt und dass er mindestens ein Blob enthält.
* Navigieren Sie in Ihrem Browser mithilfe der benutzerdefinierten Domäne zur Adresse des Blobs. Wenn die benutzerdefinierte Domäne beispielsweise `cdn.contoso.com` ist, ähnelt die URL eines zwischengespeicherten Blobs der folgenden URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Weitere Informationen
[Aktivieren des CDN (Content Delivery Network) für Azure](cdn-create-new-endpoint.md)  




<!--HONumber=Nov16_HO3-->


