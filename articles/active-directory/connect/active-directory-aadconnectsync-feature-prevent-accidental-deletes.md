---
title: "Azure AD Connect-Synchronisierung: Verhindern von versehentlichen Löschungen | Microsoft Docs"
description: "In diesem Thema wird die Funktion zum Verhindern von versehentlichen Löschungen (Verhindern versehentlicher Löschvorgänge) in Azure AD Connect beschrieben."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/04/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: aa20b20c86763791eb579883b5273ea79cc714b5
ms.openlocfilehash: 449cfa39e17a3b753cf3e267aed2d6a439fc5f36


---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect-Synchronisierung: Verhindern von versehentlichen Löschvorgängen
In diesem Thema wird die Funktion zum Verhindern von versehentlichen Löschungen (Verhindern versehentlicher Löschvorgänge) in Azure AD Connect beschrieben.

Bei der Installation von Azure AD Connect wird die Funktion zum Schutz vor unbeabsichtigtem Löschen standardmäßig aktiviert und so konfiguriert, das Exporte mit mehr als 500 Löschungen unterbunden werden. Diese Funktion dient zum Schutz vor unbeabsichtigten Konfigurationsänderungen und Änderungen an Ihrem lokalen Verzeichnis, die sich auf viele Benutzer und andere Objekte auswirken würden.

Häufige Szenarios für Löschvorgänge sind beispielsweise:

* Änderungen an der [Filterung](active-directory-aadconnectsync-configure-filtering.md), bei denen die Auswahl einer gesamten [Organisationseinheit](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) oder [Domäne](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) aufgehoben wird.
* Das Löschen aller Objekte in einer Organisationseinheit.
* Das Umbenennen einer Organisationseinheit, sodass alle darin enthaltenen Objekte als außerhalb des Synchronisierungsbereichs liegend betrachtet werden.

Der Standardwert von 500 Objekten kann mit PowerShell mithilfe von `Enable-ADSyncExportDeletionThreshold`geändert werden. Sie sollten diesen Wert entsprechend der Größe Ihres Unternehmens konfigurieren. Da der Synchronisierungsplaner alle 30 Minuten ausgeführt wird, entspricht der Wert der Anzahl von Löschvorgängen, die innerhalb von 30 Minuten erfolgen.

Wenn zu viele Löschungen in Azure AD exportiert werden sollen, wird der Export angehalten, und Sie erhalten eine E-Mail-Nachricht wie die folgende:

![E-Mail zum Verhindern von versehentlichen Löschungen](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hallo (Ansprechpartner für Technik), am (Datum und Uhrzeit) hat der Dienst für die Identitätssynchronisierung festgestellt, dass die Anzahl von Löschvorgängen den für (Name der Organisation) konfigurierten Schwellenwert überschritten hat. Es wurden insgesamt (Anzahl) Objekte bei dieser Ausführung der Identitätssynchronisierung zum Löschen gesendet. Dies entspricht dem konfigurierten Schwellenwert für Löschungen von (Anzahl) Objekten bzw. überschreitet ihn. Sie müssen vor dem Fortfahren bestätigen, dass diese Löschvorgänge durchgeführt werden sollen. Weitere Informationen zu dem in dieser E-Mail-Nachricht genannten Fehler finden Sie unter „Verhindern von zufälligem Löschen“.*
> 
> 

Sie können den Status `stopped-deletion-threshold-exceeded` auch auf der **Synchronization Service Manager** -Benutzeroberfläche für das Exportprofil sehen.
![Synchronization Service Manager-Benutzeroberfläche zum Verhindern von versehentlichen Löschungen](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Wenn Sie diese Nachricht unerwartet erhalten haben, untersuchen Sie die Grunde dafür, und ergreifen Sie die nötigen Maßnahmen, um das Problem zu beheben. Führen Sie die folgenden Schritte aus, um zu ermitteln, welche Objekte gelöscht werden sollen:

1. Starten Sie den **Synchronisierungsdienst** über das Startmenü.
2. Gehen Sie zu **Connectors**.
3. Wählen Sie den Connector mit dem Typ **Azure Active Directory**.
4. Wählen Sie unter **Actions** (Aktionen) auf der rechten Seite **Search Connector Space** (Connectorbereich suchen).
5. Wählen Sie im Popupfenster unter **Scope** (Bereich) die Option **Disconnected Since** (Getrennt seit) aus, und wählen Sie einen Zeitpunkt in der Vergangenheit. Klicken Sie auf **Suchen**. Auf dieser Seite wird eine Übersicht über alle zu löschenden Objekte angezeigt. Klicken Sie auf die einzelnen Objekte, um zusätzliche Informationen dazu zu erhalten. Sie können auch auf **Spalteneinstellung** klicken, um zusätzliche Attribute hinzuzufügen, die im Raster angezeigt werden.

![Suche Connectorbereich](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Wenn alle Löschvorgänge gewünscht sind, gehen Sie folgendermaßen vor:

1. Um den Schutz vorübergehend zu deaktivieren und diese Löschvorgänge zuzulassen, führen Sie folgendes PowerShell-Cmdlet aus: `Disable-ADSyncExportDeletionThreshold`. Geben Sie das Konto und das Kennwort eines globalen Azure AD-Administrators ein.
   ![Anmeldeinformationen](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. Lassen Sie Azure Active Directory Connector weiterhin ausgewählt, wählen Sie die Aktion **Run** (Ausführen) und anschließend **Export** (Exportieren) aus.
3. Führen Sie zum erneuten Aktivieren des Schutzes folgendes PowerShell-Cmdlet aus: `Enable-ADSyncExportDeletionThreshold`.

## <a name="next-steps"></a>Nächste Schritte
**Übersichtsthemen**

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](active-directory-aadconnectsync-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)




<!--HONumber=Nov16_HO3-->


