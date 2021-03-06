---
title: Benutzerstatus in Microsoft Azure Multi-Factor Authentication
description: "Erfahren Sie etwas über Benutzerstatus in Azure MFA."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5c8b747ef8751f9556e0d23a97ec320cbf3265e6


---
# <a name="user-states-in-azure-multi-factor-authentication"></a>Benutzerstatus in Azure Multi-Factor Authentication
Benutzerkonten in Azure Multi-Factor Authentication können die folgenden drei Zustände aufweisen:

| Zustand | Beschreibung | Nicht-Browser-Apps betroffen | Hinweise |
|:---:|:---:|:---:|:---:|
| Deaktiviert |Der Standardzustand für einen neuen Benutzer, der nicht für Multi-Factor Authentication registriert ist. |Nein |Der Benutzer verwendet keine Multi-Factor Authentication. |
| Aktiviert |Der Benutzer wurde für Multi-Factor Authentication registriert. |Nein.  Sie werden weiterhin ausgeführt, bis die Registrierung abgeschlossen ist. |Der Benutzer ist aktiviert, seine Registrierung ist jedoch nicht abgeschlossen. Er wird beim nächsten Anmelden aufgefordert, den Vorgang abzuschließen. |
| Erzwungen |Der Benutzer wurde registriert und hat die Registrierung für die Verwendung von Multi-Factor Authentication abgeschlossen. |Ja.  Für Apps sind App-Kennwörter erforderlich. |Der Benutzer hat die Registrierung abgeschlossen oder nicht abgeschlossen. Wenn er die Registrierung abgeschlossen hat, verwendet er anschließend Multi-Factor Authentication. Andernfalls wird der Benutzer beim nächsten Anmelden aufgefordert, den Vorgang abzuschließen. |

## <a name="changing-a-user-state"></a>Ändern eines Benutzerstatus
Ein Benutzerstatus ändert sich abhängig davon, ob er für MFA eingerichtet wurde und ob der Benutzer den Vorgang abgeschlossen hat.  Wenn Sie MFA für einen Benutzer aktivieren, ändert sich der Benutzerstatus von „Deaktiviert“ in „Aktiviert“.  Sobald der Benutzer, dessen Status in „Aktiviert“ geändert wurde, sich anmeldet und den Vorgang abschließt, ändert der Zustand sich in „Erzwungen“.  

### <a name="to-view-a-users-state"></a>So zeigen Sie den Status eines Benutzers an
- - -
1. Melden Sie sich beim **klassischen Azure-Portal** als Administrator an.
2. Klicken Sie im linken Bereich auf **Active Directory**.
3. Klicken Sie unter **Verzeichnis** auf das Verzeichnis für den Benutzer, den Sie aktivieren möchten.
   ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. Klicken Sie oben auf **Benutzer**.
5. Klicken Sie unten auf der Seite auf **Multi-Factor Auth verwalten**.
   ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. Dadurch wird eine neue Browserregisterkarte geöffnet.  Sie können den Benutzerstatus anzeigen.
   ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="to-change-the-state-from-disabled-to-enabled"></a>So ändern Sie den Status von „Deaktiviert“ in „Aktiviert“
1. Melden Sie sich beim **klassischen Azure-Portal** als Administrator an.
2. Klicken Sie im linken Bereich auf **Active Directory**.
3. Klicken Sie unter **Verzeichnis** auf das Verzeichnis für den Benutzer, den Sie aktivieren möchten.
   ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. Klicken Sie oben auf **Benutzer**.
5. Klicken Sie unten auf der Seite auf **Multi-Factor Auth verwalten**.
   ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. Dadurch wird eine neue Browserregisterkarte geöffnet.  Suchen Sie den Benutzer, für den Sie Multi-Factor Authentication aktivieren möchten. Sie müssen möglicherweise oben die Ansicht ändern. Vergewissern Sie sich, dass der Status **Deaktiviert** lautet.
   ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7. Aktivieren Sie das Kontrollkästchen **** neben dem gewünschten Namen.
8. Klicken Sie auf der rechten Seite auf **Aktivieren**.
   ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/user1.png)
9. Klicken Sie auf **Multi-Factor Auth aktivieren**.
   ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/enable2.png)
10. Sie werden feststellen, dass sich der Status des Benutzers von **Deaktiviert** in **Aktiviert** geändert hat.
    ![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/user.png)
11. Es wird empfohlen, dass Sie den Benutzer per E-Mail benachrichtigen, nachdem Sie ihn aktiviert haben.  Diese sollte auch eine Information dazu enthalten, wie Benutzer ihre Nicht-Browser-Apps verwenden können, um eine Sperrung zu vermeiden.

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>So ändern Sie den Status von „Aktiviert/Erzwungen“ in „Deaktiviert“
1. Melden Sie sich beim **klassischen Azure-Portal** als Administrator an.
2. Klicken Sie im linken Bereich auf **Active Directory**.
3. Klicken Sie unter **Verzeichnis** auf das Verzeichnis für den Benutzer, den Sie aktivieren möchten.
   ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. Klicken Sie oben auf **Benutzer**.
5. Klicken Sie unten auf der Seite auf **Multi-Factor Auth verwalten**.
   ![Auf „Verzeichnis“ klicken](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. Dadurch wird eine neue Browserregisterkarte geöffnet.  Suchen Sie den Benutzer, den Sie deaktivieren möchten. Sie müssen möglicherweise oben die Ansicht ändern. Stellen Sie sicher, dass der Status entweder **Aktiviert** oder **Erzwungen** lautet.
7. Aktivieren Sie das Kontrollkästchen **** neben dem gewünschten Namen.
8. Klicken Sie auf der rechten Seite auf **Deaktivieren**.
   ![Benutzer deaktivieren](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
9. Sie werden aufgefordert, dies zu bestätigen.  Klicken Sie auf **Ja**.
   ![Benutzer deaktivieren](./media/multi-factor-authentication-get-started-user-states/userstate3.png)
10. Es sollte angezeigt werden, dass der Vorgang erfolgreich abgeschlossen wurde.  Klicken Sie auf **Schließen.**
    ![Benutzer deaktivieren](./media/multi-factor-authentication-get-started-user-states/userstate4.png)




<!--HONumber=Nov16_HO3-->


