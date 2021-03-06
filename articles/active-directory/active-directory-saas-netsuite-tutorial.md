---
title: 'Tutorial: Azure Active Directory-Integration mit NetSuite | Microsoft-Dokumentation'
description: "Hier erfahren Sie, wie Sie NetSuite mit Azure Active Directory verwenden, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2016
ms.author: asmalser
translationtype: Human Translation
ms.sourcegitcommit: 6b77e338e1c7f0f79ea3c25b0b073296f7de0dcf
ms.openlocfilehash: 74ef08108a5ff27a50f928781f906b6d769f1085


---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Tutorial: Azure Active Directory-Integration mit NetSuite
In diesem Tutorial erfahren Sie, wie Sie Ihre NetSuite-Umgebung mit Azure Active Directory (Azure AD) verbinden. Sie erfahren, wie Sie einmaliges Anmelden bei NetSuite konfigurieren, die automatisierte Benutzerbereitstellung aktivieren und Benutzern den Zugriff auf NetSuite zuweisen. 

## <a name="prerequisites"></a>Voraussetzungen
1. Um über das [klassische Azure-Portal](https://manage.windowsazure.com)auf Azure Active Directory zuzugreifen, müssen Sie über ein gültiges Azure-Abonnement verfügen.
2. Sie benötigen Administratorzugriff auf ein [NetSuite](http://www.netsuite.com/portal/home.shtml)-Abonnement. Sie können ein kostenloses Testkonto für einen der Dienste verwenden.

## <a name="step-1-add-netsuite-to-your-directory"></a>Schritt 1: Hinzufügen von NetSuite zu Ihrem Verzeichnis
1. Klicken Sie im linken Navigationsbereich des [klassischen Azure-Portals](https://manage.windowsazure.com)auf **Active Directory**.
   
    ![Wählen Sie im linken Navigationsbereich "Active Directory".][0]
2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis aus, dem Sie NetSuite hinzufügen möchten.
3. Klicken Sie im oberen Menü auf **Anwendungen** .
   
    ![Klicken Sie auf "Anwendungen".][1]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Klicken Sie auf "Hinzufügen", um eine neue Anwendung hinzufügen.][2]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Klicken Sie auf "Anwendung aus dem Katalog hinzufügen".][3]
6. Geben Sie im **Suchfeld** den Suchbegriff **NetSuite** ein. Wählen Sie in den Ergebnissen die Option **NetSuite** aus, und klicken Sie auf **Fertig stellen**, um die Anwendung hinzuzufügen.
   
    ![Hinzufügen von NetSuite][4]
7. Sie sollten jetzt die Seite „Schnellstart“ für NetSuite sehen:
   
    ![Seite „Schnellstart“ für NetSuite in Azure AD][5]

## <a name="step-2-enable-single-sign-on"></a>Schritt 2: Aktivieren der einmaligen Anmeldung
1. Klicken Sie in Azure AD auf der Seite „Schnellstart“ für NetSuite auf die Schaltfläche **Einmaliges Anmelden konfigurieren**.
   
    ![Schaltfläche zum Konfigurieren der einmaligen Anmeldung][6]
2. Ein Dialogfeld wird geöffnet, und Sie sehen einen Bildschirm mit der Frage „Wie sollen sich Benutzer bei NetSuite anmelden?“. Wählen Sie **Azure AD – einmaliges Anmelden**, und klicken Sie dann auf **Weiter**.
   
    ![Wählen Sie "Azure AD – einmaliges Anmelden".][7]
   
   > [!NOTE]
   > Um weitere Informationen zu den verschiedenen Optionen für die einmalige Anmeldung zu erhalten, [klicken Sie hier](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)
   > 
   > 
3. Geben Sie auf der Seite **App-Einstellungen konfigurieren** im Feld **Antwort-URL** Ihre NetSuite-Mandanten-URL in einem der folgenden Formate ein:
   
   * `https://<tenant-name>.netsuite.com/saml2/acs`
   * `https://<tenant-name>.na1.netsuite.com/saml2/acs`
   * `https://<tenant-name>.na2.netsuite.com/saml2/acs`
   * `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
   * `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
   * `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`
     
     ![Geben Sie Ihre Mandanten-URL ein.][8]
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für NetSuite** auf **Metadaten herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer.
   
    ![Laden Sie die Metadaten herunter.][9]
5. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich bei Ihrer Netsuite-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf der Symbolleiste am oberen Rand der Seite auf **Setup** und dann auf **Setup-Manager**.
   
    ![Wechseln Sie zum Setup-Manager.][10]
7. Wählen Sie in der Liste **Setupaufgaben** die Option **Integration** aus.
   
    ![Wechseln Sie zur Integration.][11]
8. Klicken Sie im Abschnitt **Authentifizierung verwalten** auf **Einmalige Anmeldung für SAML**.
   
    ![Wechseln Sie zur einmaligen Anmeldung für SAML.][12]
9. Führen Sie auf der Seite **SAML-Setup** die folgenden Schritte aus:
   
   * Kopieren Sie in Azure Active Directory den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in NetSuite in das Feld **Identity Provider Login Page** (Anmeldeseite für Identitätsanbieter) ein.
     
       ![Die SAML-Setup-Seite.][13]
   * Wählen Sie in NetSuite **Primary Authentication Method** (Primäre Authentifizierungsmethode).
   * Wählen Sie im Feld mit der Bezeichnung **SAMLV2-Identitätsanbietermetadaten** die Option **IDP-Metadatendatei hochladen** aus. Klicken Sie dann zum Hochladen der Metadatendatei, die Sie in Schritt 4 heruntergeladen haben, auf **Durchsuchen** .
     
       ![Laden Sie die Metadaten hoch.][16]
   * Klicken Sie auf **Senden**.
10. Aktivieren Sie in Azure AD das Kontrollkästchen zur Bestätigung der Konfiguration der einmaligen Anmeldung, um das Zertifikat zu aktivieren, das Sie in NetSuite hochgeladen haben. Klicken Sie auf **Weiter**.
    
     ![Aktivieren Sie das Kontrollkästchen zur Bestätigung.][14]
11. Geben Sie auf der letzten Seite des Dialogfelds eine E-Mail-Adresse ein, wenn Sie per E-Mail Fehler- und Warnmeldungen im Zusammenhang mit der Wartung dieser Konfiguration für die einmalige Anmeldung erhalten möchten. 
    
    ![Geben Sie Ihre E-Mail-Adresse ein.][15]
12. Klicken Sie auf **Fertig stellen** , um das Dialogfeld zu schließen. Klicken Sie anschließend am oberen Rand der Seite auf **Attribute** .
    
    ![Klicken Sie auf Attribute.][17]
13. Klicken Sie auf **Benutzerattribut hinzufügen**.
    
    ![Klicken Sie auf "Benutzerattribut hinzufügen".][18]
14. Geben Sie im Feld **Attributname** die Zeichenfolge `account` ein. Geben Sie in das Feld **Attributwert** Ihre Konto-ID für NetSuite ein. Beachten Sie die folgenden Anleitungen, um Ihre Konto-ID zu finden:
    
    ![Fügen Sie Ihre NetSuite-Konto-ID hinzu.][19]
    
    * Klicken Sie in NetSuite im oberen Navigationsmenü auf **Setup**.
    * Klicken Sie dann im linken Navigationsmenü im Abschnitt **Setupaufgaben** auf den Abschnitt **Integration**, und klicken Sie auf **Webdiensteeinstellungen**.
    * Kopieren Sie Ihre Konto-ID für NetSuite, und fügen Sie sie in Azure AD in das Feld **Attributwert** ein.
      
        ![Rufen Sie Ihre Konto-ID ab.][20]
15. Klicken Sie in Azure AD auf **Fertig stellen** , um das Hinzufügen des SAML-Attributs abzuschließen. Klicken Sie dann im unteren Menü auf **Änderungen übernehmen** .
    
    ![Speichern Sie die Änderungen.][21]
16. Bevor Benutzer die einmalige Anmeldung in NetSuite ausführen können, müssen ihnen zunächst die entsprechenden Berechtigungen in NetSuite zugewiesen werden. Befolgen Sie die folgenden Anweisungen, um diese Berechtigungen zuzuweisen.
    
    * Klicken Sie im oberen Navigationsmenü auf **Setup** und dann auf **Setup-Manager**.
      
        ![Wechseln Sie zum Setup-Manager.][10]
    * Wählen Sie im linken Navigationsmenü **Benutzer/Rollen** aus, und klicken Sie auf **Rollen verwalten**.
      
        ![Wechseln Sie zum Verwalten von Rollen.][22]
    * Klicken Sie auf **Neue Rolle**.
    * Geben Sie einen Namen** **für die neue Rolle ein, und aktivieren Sie das Kontrollkästchen **Nur einmaliges Anmelden**.
      
        ![Benennen Sie die neue Rolle.][23]
    * Klicken Sie auf **Speichern**.
    * Klicken Sie im oberen Menü auf **Berechtigungen**. Klicken Sie dann auf **Setup**.
      
        ![Wechseln Sie zu Berechtigungen.][24]
    * Wählen Sie **Set Up SAM Single Sign-on** (Einmaliges Anmelden für SAM einrichten), und klicken Sie dann auf **Hinzufügen**.
    * Klicken Sie auf **Speichern**.
    * Klicken Sie im oberen Navigationsmenü auf **Setup** und dann auf **Setup-Manager**.
      
        ![Wechseln Sie zum Setup-Manager.][10]
    * Wählen Sie im linken Navigationsmenü **Benutzer/Rollen** aus, und klicken Sie auf **Benutzer verwalten**.
      
        ![Wechseln Sie zum Verwalten von Benutzern.][25]
    * Wählen Sie einen Testbenutzer. Klicken Sie dann auf **Bearbeiten**.
      
        ![Wechseln Sie zum Verwalten von Benutzern.][26]
    * Wählen Sie im Dialogfeld „Rollen“ die Rolle aus, die Sie erstellt haben, und klicken Sie auf **Hinzufügen**.
      
        ![Wechseln Sie zum Verwalten von Benutzern.][27]
    * Klicken Sie auf **Speichern**.
17. Weiter unten im Abschnitt [Zuweisen von Benutzern zu NetSuite](#step-4-assign-users-to-netsuite) erfahren Sie, wie Sie Ihre Konfiguration testen können.

## <a name="step-3-enable-automated-user-provisioning"></a>Schritt 3: Aktivieren der automatisierten Benutzerbereitstellung
> [!NOTE]
> Standardmäßig werden die bereitgestellten Benutzer der Stammniederlassung Ihrer NetSuite-Umgebung hinzugefügt.
> 
> 

1. Klicken Sie in Azure Active Directory auf der Seite „Schnellstart“ für NetSuite auf **Benutzerbereitstellung konfigurieren**.
   
    ![Benutzerbereitstellung konfigurieren][28]
2. Geben Sie in dem nun geöffneten Dialogfeld Ihre Administratoranmeldeinformationen für NetSuite ein, und klicken Sie auf **Weiter**.
   
    ![Geben Sie Ihre NetSuite-Admin-Anmeldeinformationen ein.][29]
3. Geben Sie auf der Bestätigungsseite Ihre E-Mail-Adresse ein, um Benachrichtigungen über Fehler bei der Bereitstellung zu erhalten.
   
    ![Bestätigen Sie den Vorgang.][30]
4. Klicken Sie auf **Fertig stellen** , um das Dialogfeld zu schließen.

## <a name="step-4-assign-users-to-netsuite"></a>Schritt 4: Zuweisen von Benutzern zu NetSuite
1. Um Ihre Konfiguration zu testen, beginnen Sie mit dem Erstellen eines neuen Testkontos im Verzeichnis.
2. Klicken Sie auf der Seite „Schnellstart“ für NetSuite auf die Schaltfläche **Benutzer zuweisen**.
   
    ![Klicken Sie auf "Benutzer zuweisen".][31]
3. Wählen Sie Ihren Testbenutzer aus, und klicken Sie am unteren Bildschirmrand auf die Schaltfläche **Zuweisen** :
   
   * Wenn Sie die automatisierte Benutzerbereitstellung nicht aktiviert haben, werden Sie aufgefordert, Folgendes zu bestätigen:
     
        ![Zuweisung bestätigen][32]
   * Wenn Sie die automatisierte Benutzerbereitstellung aktiviert haben, werden Sie aufgefordert, zu definieren, welche Art von Rolle der Benutzer in NetSuite erhalten soll. Neu bereitgestellte Benutzer sollten in Ihrer NetSuite-Umgebung nach einigen Minuten angezeigt werden.
4. Um Ihre Einstellungen für die einmalige Anmeldung zu testen, öffnen Sie den Zugriffsbereich unter [https://myapps.microsoft.com](https://myapps.microsoft.com/), melden Sie sich beim Testkonto an, und klicken Sie auf **NetSuite**.

## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png



<!--HONumber=Dec16_HO2-->


