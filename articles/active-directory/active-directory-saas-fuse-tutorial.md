---
title: 'Tutorial: Azure Active Directory-Integration mit Fuse | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Fuse konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 5ef34f58-863a-4b37-875c-e8efa3e18bb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 7183f3c93b991d3ed350be7c32564d9acaa7da51


---
# <a name="tutorial-azure-active-directory-integration-with-fuse"></a>Tutorial: Azure Active Directory-Integration mit Fuse
Dieses Tutorial soll Ihnen zeigen, wie Sie Fuse in Azure Active Directory (Azure AD) integrieren können.

Die Integration von Fuse in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Fuse hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Fuse anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Fuse konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Fuse-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Fuse aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-fuse-from-the-gallery"></a>Hinzufügen von Fuse aus dem Katalog
Zum Konfigurieren der Integration von Fuse in Azure AD müssen Sie Fuse aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Fuse aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **Fuse**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_01.png)
7. Wählen Sie im Ergebnisbereich **Fuse** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll veranschaulicht werden, wie basierend auf einem Testbenutzer namens „Britta Simon“ das einmalige Anmelden von Azure AD in Fuse konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Fuse als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Fuse muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD als Benutzernamen**** in Fuse zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Fuse müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Fuse-Testbenutzers](#creating-a-fuse-test-user)** , um eine Entsprechung von Britta Simon in Fuse zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer Fuse-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Fuse die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Fuse** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Fuse anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_04.png) 
   
    a. Geben Sie im Textfeld „Anmelde-URL“ die URL, die von Ihren Benutzern zur Anmeldung bei der Fuse-Anwendung verwendet wird, nach folgendem Muster ein: **https://\<tenant name\>.fusion-universal.com/**.
   
   > [!NOTE]
   > Falls Sie Ihre Anmelde-URL nicht kennen, wenden Sie sich an das [Supportteam von Fuse](mailto:support@fusion-universal.com) , um sie zu erhalten.
   > 
   > 
   
    b. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Fuse** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_05.png) 
   
    a. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie es auf Ihrem Computer.
   
    b. Kopieren Sie die **Aussteller-URL**, die **Dienst-URL für einmalige Anmeldung** und die **Dienst-URL für einmalige Abmeldung**.
5. Lassen Sie das einmalige Anmelden (SSO, Single Sign-On) für Ihre Anwendung konfigurieren. Wenden Sie sich dazu unter **support@fusion-universal.com** an das Supportteam von Fuse, und fügen Sie die heruntergeladene Zertifikatsdatei an. Geben Sie außerdem die **Aussteller-URL**, die **Dienst-URL für einmalige Anmeldung** und die **Dienst-URL für einmalige Abmeldung** an.
6. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

Wählen Sie in der Benutzerliste **Britta Simon**aus.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_06.png)
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_07.png)
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_08.png)
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-fuse-test-user"></a>Erstellen eines Fuse-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Fuse. Fuse unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Fuse ein neuer Benutzer erstellt. [Konfigurieren der einmaligen Anmeldung in Azure AD](#configuring-azure-ad-single-single-sign-on).

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf Fuse erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon Fuse zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste den Eintrag **Fuse**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
In diesem Abschnitt soll Ihre Azure AD-Konfiguration für das einmalige Anmelden mithilfe des Zugriffsbereichs getestet werden.

Wenn Sie im Zugriffsbereich auf die Kachel „Fuse“ klicken, sollten Sie automatisch bei Ihrer Fuse-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


