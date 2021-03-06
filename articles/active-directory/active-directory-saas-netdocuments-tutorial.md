---
title: 'Tutorial: Azure Active Directory-Integration mit NetDocuments | Microsoft Docs'
description: "Erfahren Sie, wie Sie NetDocuments mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e35d086cdb59f52db95d16429c7266934813fe24


---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Tutorial: Azure Active Directory-Integration mit NetDocuments
In diesem Tutorial wird die Integration von Azure und NetDocuments erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen NetDocuments-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie NetDocuments zugewiesen haben, mittels einmaliger Anmeldung auf Ihrer NetDocuments-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für NetDocuments
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Scenario")

## <a name="enabling-the-application-integration-for-netdocuments"></a>Aktivieren der Anwendungsintegration für NetDocuments
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für NetDocuments aktivieren.

### <a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für NetDocuments
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **NetDocuments** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **NetDocuments** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![NetDocuments](./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei NetDocuments zu authentifizieren.  
Zum Konfigurieren des einmaligen Anmeldens für NetDocuments müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **NetDocuments** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei NetDocuments anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Configure Single Sign-On")
3. Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus:
   
   ![App-URL konfigurieren](./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Configure App URL")
   
   1. Geben Sie im Textfeld **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer NetDocuments-Anwendung verwendete URL ein (z.B.: „*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*“).
   2. Geben Sie im Textfeld **NetDocuments-Antwort-URL** den gleichen Wert wie in das Textfeld **Anmelde-URL** ein.  
      
      > [!NOTE]
      > Den richtigen Wert finden Sie am Ende des Dialogfelds **Identitätsverbund** (siehe Screenshot für Schritt 9).
      > 
      > 
   3. Klicken Sie auf **Weiter**
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für NetDocuments** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der NetDocuments-Unternehmenswebsite als Administrator an.
6. Wechseln Sie zu **Administrator**.
7. Klicken Sie auf **Benutzer und Gruppen hinzufügen und entfernen**.
   
   ![Repository](./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repository")
8. Klicken Sie auf **Erweiterte Authentifizierungsoptionen konfigurieren**.
   
   ![Erweiterte Authentifizierungsoptionen konfigurieren](./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Configure advanced authentication options")
9. Führen Sie im Dialogfeld **Identitätsverbund** die folgenden Schritte aus:
   
   ![Identitätsverbund](./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Federated Identitty")
   
   1. Wählen Sie als **Servertyp für Identitätsverbund** die Option **Active Directory-Verbunddienste** aus.
   2. Klicken Sie auf **Datei auswählen**, um die heruntergeladene Metadatendatei hochzuladen.
   3. Klicken Sie auf **OK**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Configure Single Sign-On")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei NetDocuments anmelden können, müssen sie in NetDocuments bereitgestellt werden.  
Im Fall von NetDocuments ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei Ihrer **NetDocuments** -Unternehmenswebsite als Administrator an.
2. Klicken Sie oben im Menü auf **Administrator**.
   
   ![Administrator](./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Admin")
3. Klicken Sie auf **Benutzer und Gruppen hinzufügen und entfernen**.
   
   ![Repository](./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repository")
4. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse eines gültigen Azure Active Directory-Kontos ein, das Sie bereitstellen möchten, und klicken Sie auf **Benutzer hinzufügen**.
   
   ![E-Mail-Adresse](./media/active-directory-saas-netdocuments-tutorial/IC795053.png "Email Address")
   
   > [!NOTE]
   > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
   > 
   > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von NetDocuments-Benutzerkonten oder mithilfe der von NetDocuments bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>So weisen Sie NetDocuments Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **NetDocuments** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


