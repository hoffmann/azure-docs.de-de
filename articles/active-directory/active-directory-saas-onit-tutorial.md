---
title: 'Tutorial: Azure Active Directory-Integration mit Onit | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Onit mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 80c727ba687e11f7407339b57bc1f6b0c091dd0f


---
# <a name="tutorial-azure-active-directory-integration-with-onit"></a>Tutorial: Azure Active Directory-Integration mit Onit
In diesem Tutorial wird die Integration von Azure und Onit erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Onit-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können socj die Azure AD-Benutzer, die Sie Onit zugewiesen haben, wie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)beschrieben mittels einmaliger Anmeldung bei Ihrer Onit-Unternehmenswebsite anmelden (durch den Dienstanbieter initiierte Anmeldung).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Onit
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-onit-tutorial/IC791166.png "Scenario")

## <a name="enabling-the-application-integration-for-onit"></a>Aktivieren der Anwendungsintegration für Onit
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Onit aktivieren.

### <a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Onit
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-onit-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-onit-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-onit-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-onit-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **Onit** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-onit-tutorial/IC791167.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Onit** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Onit](./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Onit zu authentifizieren.  
Zum Konfigurieren des einmaligen Anmeldens für Onit müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

Die Onit-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen.  
Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![Einmaliges Anmelden](./media/active-directory-saas-onit-tutorial/IC791168.png "Single Sign-On")

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Onit** im Menü oben auf **Attribute**, um das Dialogfeld **SAML-Tokenattribute** zu öffnen.
   
   ![Attribute](./media/active-directory-saas-onit-tutorial/IC791169.png "Attributes")
2. Führen Sie zum Hinzufügen der erforderlichen Attributzuordnungen die folgenden Schritte aus:

    |Attributname|Attributwert|
    |---|---|
    |Name|User.userprincipalname|
    |E-Mail|User.mail|

    1.  Klicken Sie für jede Datenzeile in der obigen Tabelle auf **Benutzerattribut hinzufügen**.
    2.  Geben Sie im Textfeld **Attributname** den für die Zeile angezeigten Attributnamen ein.
    3.  Wählen Sie in der Liste **Attributwert** den für die Zeile angezeigten Attributwert aus.
    4.  Klicken Sie auf **Fertig stellen**.

1. Klicken Sie auf **Änderungen übernehmen**.
2. Klicken Sie in Ihrem Browser auf **Zurück**, um erneut das Dialogfeld **Schnellstart** zu öffnen.
3. Klicken Sie auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-onit-tutorial/IC791170.png "Configure Single Sign-On")
4. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Onit** anmelden? die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-onit-tutorial/IC791171.png "Configure Single Sign-On")
5. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Onit-Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer Onit-Anwendung verwendete URL ein (z.B. „*https://ms-sso-test.onit.com*“), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-onit-tutorial/IC791172.png "Configure App URL")
6. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Onit** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-onit-tutorial/IC791173.png "Configure Single Sign-On")
7. Melden Sie sich in einem anderen Webbrowserfenster bei der Onit-Unternehmenswebsite als Administrator an.
8. Klicken Sie oben im Menü auf **Verwaltung**.
   
   ![Verwaltung](./media/active-directory-saas-onit-tutorial/IC791174.png "Administration")
9. Klicken Sie auf **Unternehmen bearbeiten**.
   
   ![Unternehmen bearbeiten](./media/active-directory-saas-onit-tutorial/IC791175.png "Edit Corporation")
10. Klicken Sie auf die Registerkarte **Sicherheit** .
    
    ![Unternehmensinformationen bearbeiten](./media/active-directory-saas-onit-tutorial/IC791176.png "Edit Company Information")
11. Führen Sie im Abschnitt **Sicherheit** die folgenden Schritte aus:
    
    ![Einmaliges Anmelden](./media/active-directory-saas-onit-tutorial/IC791177.png "Single Sign-On")
    
    1. Wählen Sie als **Authentifizierungsstrategie** die Option **Einmaliges Anmelden und Kennwort**.
    2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Onit** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Idp Target URL** ein.
    3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren** für Onit den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Idp Logout URL** ein.
    4. Kopieren Sie den **Fingerabdruckwert** aus dem exportierten Zertifikat, und fügen Sie ihn in das Textfeld **Fingerabdruck des IDP-Zertifikats (SHA1)** ein.  
       
       > [!TIP]
       > Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)
       > 
       > 
    5. Wählen Sie für **SSO-Typ** die Option **SAML** aus.
    6. Geben Sie im Textfeld **Text der SSO-Anmeldeschaltfläche** den gewünschten Text ein.
    7. Wählen Sie **Anmeldung mit SSO: Erforderlich für die folgenden Domänen/Benutzer**, geben Sie die E-Mail-Adresse eines Testbenutzers in das entsprechende Textfeld ein, und klicken Sie dann auf **Aktualisieren**.
       ![Unternehmen bearbeiten](./media/active-directory-saas-onit-tutorial/IC791178.png "Edit Corporation")
12. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-onit-tutorial/IC791179.png "Configure Single Sign-On")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Onit anmelden können, müssen sie in Onit bereitgestellt werden.  
Im Fall von Onit ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der **Onit** -Unternehmenswebsite als Administrator an.
2. Klicken Sie auf **Benutzer hinzufügen**.
   
   ![Verwaltung](./media/active-directory-saas-onit-tutorial/IC791180.png "Administration")
3. Führen Sie auf der Dialogfeldseite **Benutzer hinzufügen** die folgenden Schritte aus:
   
   ![Benutzer hinzufügen](./media/active-directory-saas-onit-tutorial/IC791181.png "Add User")
   
   1. Geben Sie **Name** und **E-Mail-Adresse** eines gültigen AAD-Benutzerkontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   2. Klicken Sie auf **Erstellen**.  
      
      > [!NOTE]
      > Der Besitzer des Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
      > 
      > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Onit-Benutzerkonten oder mithilfe der von Onit bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-onit-perform-the-following-steps"></a>So weisen Sie Onit Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Onit** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-onit-tutorial/IC791182.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-onit-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


