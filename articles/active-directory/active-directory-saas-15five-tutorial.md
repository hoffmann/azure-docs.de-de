---
title: 'Tutorial: Azure Active Directory-Integration mit 15Five | Microsoft-Dokumentation'
description: "Erfahren Sie, wie Sie 15Five mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 2fb301c2-7d7a-4046-8ee1-7dc9e7684806
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 7a0a300f505d9012471679ac27373944f07fdba3
ms.openlocfilehash: ce98338e6b21eb35a17f0183f409dd54d1123bb9


---
# <a name="tutorial-azure-active-directory-integration-with-15five"></a>Lernprogramm: Azure Active Directory-Integration mit 15Five
In diesem Lernprogramm wird die Integration von Azure und 15Five erläutert. Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein 15Five-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die 15Five zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer 15Five-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für 15Five
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-15five-tutorial/IC784667.png "Szenario")

## <a name="enabling-the-application-integration-for-15five"></a>Aktivieren der Anwendungsintegration für 15Five
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für 15Five aktivieren.

### <a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für 15Five
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-15five-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-15five-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-15five-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **15Five** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-15five-tutorial/IC784668.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **15Five** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![15Five](./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
   
## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei 15Five zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **15Five** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-15five-tutorial/IC784670.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei 15Five anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-15five-tutorial/IC784671.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **15Five-Anmelde-URL** Ihre URL im Format *https://company.15Five.com* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-15five-tutorial/IC784672.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für 15Five** auf **Metadaten herunterladen**, und senden Sie die Metadatendatei an das 15Five-Supportteam.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-15five-tutorial/IC784673.png "Einmaliges Anmelden konfigurieren")
   
   > [!NOTE]
   > Das einmalige Anmelden muss vom Supportteam von 15Five aktiviert werden.
   > 
   > 
5. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-15five-tutorial/IC784674.png "Einmaliges Anmelden konfigurieren")
   

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei 15Five anmelden können, müssen sie in 15Five bereitgestellt werden.  
Im Fall von 15Five ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der **15Five** -Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu **Manage Company**.
   
   ![Manage Company](./media/active-directory-saas-15five-tutorial/IC784675.png "Manage Company")
3. Wechseln Sie zu **People \> Add People** (Personen > Personen hinzufügen).
   
   ![Personen](./media/active-directory-saas-15five-tutorial/IC784676.png "Personen")
4. Führen Sie im Abschnitt "Add New Person" die folgenden Schritte aus:
   
   ![Neue Person hinzufügen](./media/active-directory-saas-15five-tutorial/IC784677.png "neue Person hinzufügen")
   
   1. Geben Sie in die entsprechenden Textfelder **Vorname**, **Nachname**, **Titel** und **E-Mail-Adresse** eines gültigen Azure Active Directory-Kontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Done**.
   
   > [!NOTE]
   > Der Besitzer des Azure AD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
   > 
   > 



## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-15five-perform-the-following-steps"></a>So weisen Sie 15Five Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **15Five** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-15five-tutorial/IC784678.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-15five-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO4-->


