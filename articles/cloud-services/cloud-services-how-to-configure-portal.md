---
title: Konfigurieren eines Clouddiensts (Portal) | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie Clouddienste in Azure konfigurieren. Hier erfahren Sie, wie Sie die Konfiguration für Clouddienste aktualisieren und Remotezugriff auf Rolleninstanzen konfigurieren. In diesen Beispielen wird das Azure-Portal verwendet."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: ab97962175f4498200db428736a1cbd124fac285
ms.openlocfilehash: 6c07bae5b0e6d16925da661e423cb6d80d4f3f15


---
# <a name="how-to-configure-cloud-services"></a>Konfigurieren von Clouddiensten
> [!div class="op_single_selector"]
> * [Azure-Portal](cloud-services-how-to-configure-portal.md)
> * [Klassisches Azure-Portal](cloud-services-how-to-configure.md)
> 
> 

Im Azure-Portal können Sie die am häufigsten für einen Clouddienst verwendeten Einstellungen konfigurieren. Wenn Sie die Konfigurationsdateien jedoch direkt aktualisieren möchten, laden Sie eine zu aktualisierende Konfigurationsdatei herunter, laden Sie anschließend die aktualisierte Datei hoch, und aktualisieren Sie den Clouddienst mit den Konfigurationsänderungen. In beiden Fällen wird die aktualisierte Konfiguration an alle Rolleninstanzen übermittelt.

Sie können die Instanzen Ihrer Clouddienstrollen außerdem verwalten oder eine Remotedesktopverbindung mit ihnen herstellen.

Während der Konfigurationsupdates kann Azure nur dann eine Dienstverfügbarkeit von 99,95 Prozent sicherstellen, wenn Sie mindestens zwei Rolleninstanzen für jede Rolle haben. In diesem Fall kann ein virtueller Computer Clientanforderungen verarbeiten, während der andere aktualisiert wird. Weitere Informationen finden Sie unter [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Ändern eines Clouddiensts
Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu Ihrem Clouddienst. Von hier aus können Sie zahlreiche Aspekte des Clouddiensts verwalten. 

![Seite „Einstellungen“](./media/cloud-services-how-to-configure-portal/cloud-service.png)

Über die Links **Einstellungen** oder **Alle Einstellungen** wird das Blatt **Einstellungen** geöffnet. Hier können Sie die **Eigenschaften** und die **Konfiguration** ändern, **Zertifikate** verwalten, **Warnungsregeln** einrichten und die **Benutzer** verwalten, die Zugriff auf diesen Clouddienst haben.

![Blatt mit Einstellungen für den Azure-Clouddienst](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

> [!NOTE]
> Das für den Clouddienst verwendete Betriebssystem kann nicht über das **Azure-Portal** geändert werden. Diese Einstellung können Sie nur über das [klassische Azure-Portal](http://manage.windowsazure.com/) ändern. Weitere Informationen finden Sie [hier](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).
> 
> 

## <a name="monitoring"></a>Überwachung
Sie können Ihrem Clouddienst Warnungen hinzufügen. Klicken Sie auf **Einstellungen** > **Warnungsregeln** > **Warnung hinzufügen**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

In dieser Ansicht können Sie eine Warnung einrichten. Über das Dropdownfeld **Metrik** können Sie eine Warnung für die folgenden Datentypen einrichten.

* Datenträgerlesevorgänge
* Datenträgerschreibvorgänge
* Eingehender Netzwerkverkehr
* Ausgehender Netzwerkverkehr
* CPU-Prozentsatz 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Konfigurieren der Überwachung über eine Metrikkachel
Statt die Ansicht **Einstellungen** > **Warnungsregeln** zu verwenden, können Sie auch auf eine der Metrikkacheln im Abschnitt **Überwachung** des Blatts **Clouddienst** klicken.

![Clouddienstüberwachung](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Von hier aus können Sie das in der Kachel verwendete Diagramm anpassen oder eine Warnungsregel hinzufügen.

## <a name="reboot-reimage-or-remote-desktop"></a>Neustart, Reimaging oder Remotedesktop
Momentan ist eine Konfiguration von Remotedesktop mit dem **Azure-Portal**nicht möglich. Sie können die Konfiguration stattdessen jedoch über das [klassische Azure-Portal](cloud-services-role-enable-remote-desktop.md), mithilfe von [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md) oder [Visual Studio](../vs-azure-tools-remote-desktop-roles.md) durchführen. 

Klicken Sie zunächst auf die Instanz des Clouddiensts.

![Clouddienstinstanz](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Auf dem so geöffneten Blatt können Sie eine Remotedesktopverbindung starten, die Instanz remote neu starten oder remote ein Reimaging der Instanz durchführen (mit einem neuen Image starten).

![Schaltflächen für die Clouddienstinstanz](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a>Neukonfigurieren der .cscfg-Datei
Sie müssen Ihren Clouddienst möglicherweise über die Datei mit der [Dienstkonfiguration](cloud-services-model-and-package.md#cscfg) (.cscfg) neu konfigurieren. Dazu müssen Sie Ihre .cscfg-Datei herunterladen, sie bearbeiten und wieder hochladen.

1. Klicken Sie auf das Symbol **Einstellungen** oder den Link **Alle Einstellungen**, um das Blatt **Einstellungen** zu öffnen.
   
    ![Seite „Einstellungen“](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. Klicken Sie auf das Element **Konfiguration** .
   
    ![Blatt „Konfiguration“](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. Klicken Sie auf die Schaltfläche **Herunterladen** .
   
    ![Herunterladen](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. Laden Sie nach dem Update der Dienstkonfigurationsdatei die Konfigurationsupdates hoch, und wenden Sie sie an:
   
    ![Hochladen](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
5. Wählen Sie die CSCFG-Datei aus, und klicken Sie auf **OK**.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum [Bereitstellen eines Clouddiensts](cloud-services-how-to-create-deploy-portal.md)
* [Konfigurieren eines benutzerdefinierten Domänennamens](cloud-services-custom-domain-name-portal.md)
* [Verwalten Ihres Clouddiensts](cloud-services-how-to-manage-portal.md)
* Konfigurieren von [SSL-Zertifikaten](cloud-services-configure-ssl-certificate-portal.md)




<!--HONumber=Dec16_HO2-->


