---
title: "Häufig gestellte Fragen zu Azure AD Connect | Microsoft Docs"
description: "Auf dieser Seite finden Sie häufig gestellte Fragen zu Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: aa20b20c86763791eb579883b5273ea79cc714b5
ms.openlocfilehash: d0433f7f2e88dcbf5e0969f0a6c8d2689816b2d1


---
# <a name="azure-ad-connect-faq"></a>Häufig gestellte Fragen zu Azure AD Connect
## <a name="general-installation"></a>Allgemeine Installation
**F: Ist die Installation möglich, wenn der globale Azure AD-Administrator 2FA aktiviert hat?**  
 Dies wird in den Builds ab Februar 2016 unterstützt.

**F: Gibt es eine Möglichkeit, Azure AD Connect unbeaufsichtigt zu installieren?**  
 Azure AD Connect kann nur mit dem Installationsassistenten installiert werden. Eine unbeaufsichtigte und automatische Installation wird nicht unterstützt.

**F: In meiner Gesamtstruktur ist eine Domäne nicht erreichbar. Wie installiere ich Azure AD Connect?**  
 Dies wird in den Builds ab Februar 2016 unterstützt.

**F: Funktioniert der AD DS Health-Agent im Serverkern?**  
 Ja. Nach der Installation des Agents können Sie den Registrierungsprozess abschließen, indem Sie das folgende PowerShell-Cmdlet verwenden: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Netzwerk
**F: Die maximale Länge für offene Verbindungen in meinem Netzwerk wird durch eine Firewall, ein Netzwerkgerät o. ä. eingeschränkt. Wie hoch sollte der clientseitige Schwellenwert für die Zeitüberschreitung bei der Verwendung von Azure AD Connect sein?**  
Für Netzwerksoftware, physische Geräte und alle anderen Komponenten, durch die die maximale Länge von offenen Verbindungen eingeschränkt wird, sollte ein Schwellenwert von mindestens 5 Minuten (300 Sekunden) für die Konnektivität zwischen dem Server, auf dem der Azure AD Connect-Client installiert ist, und Azure Active Directory verwendet werden. Dies gilt auch für alle zuvor veröffentlichten Microsoft Identity-Synchronisierungswerkzeuge.

**F: Werden SLDs (einteilige Domänen) unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen/Domänen mit SLDs.

**F: Werden NetBios mit punktierten Namen unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen/Domänen, deren NetBios-Name einen Punkt enthält.

## <a name="federation"></a>Verbund
**F: Was muss ich tun, wenn ich eine E-Mail erhalte, in der ich aufgefordert werde, mein Office 365-Zertifikat zu erneuern?**  
Befolgen Sie die Anweisungen, die Sie im Thema [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md) finden, um Ihr Zertifikat zu erneuern.

**F: Für die vertrauende Seite für Office 365 habe ich „Vertrauende Seite automatisch aktualisieren“ festgelegt. Muss ich bestimmte Maßnahmen ergreifen, wenn mein Tokensignaturzertifikat automatisch verlängert wird?**  
Befolgen Sie die Anweisungen im Artikel [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Environment
**F: Kann der Server nach der Installation von Azure AD umbenannt werden?**  
 Nein. Wenn Sie den Namen des Servers ändern, kann das Synchronisierungsmodul keine Verbindung zur SQL-Datenbank herstellen, und der Dienst kann nicht gestartet werden.

## <a name="identity-data"></a>Identitätsdaten
**F: Warum entspricht das UPN (UserPrincipalName)-Attribut in Azure AD dem lokalen UPN?**  
 Informationen hierzu finden Sie in diesen Artikeln:

* [Benutzernamen in Office 365, Azure oder Intune entsprechen nicht dem lokalen UPN oder der alternativen Anmelde-ID.](https://support.microsoft.com/en-us/kb/2523192)
* [Änderungen werden nicht mehr vom Azure Active Directory-Synchronisierungstool synchronisiert, wenn der UPN eines Benutzerkontos für die Verbindung mit einer anderen Verbunddomäne geändert wird.](https://support.microsoft.com/en-us/kb/2669550)

Sie können Azure AD auch so konfigurieren, dass das Synchronisierungsmodul den userPrincipalName wie unter [Features des Azure AD Connect-Synchronisierungsdiensts](active-directory-aadconnectsyncservice-features.md)beschrieben aktualisiert.

## <a name="custom-configuration"></a>Benutzerdefinierte Konfiguration
**F: Wo sind die PowerShell-Cmdlets für Azure AD Connect dokumentiert?**  
 Mit Ausnahme der auf dieser Website dokumentierten Cmdlets werden keine PowerShell-Cmdlets in Azure AD Connect zur Verwendung für Kunden unterstützt.

**F: Kann ich mithilfe von „Serverexport/Serverimport“ in *Synchronization Service Manager* die Konfiguration zwischen Servern verschieben?**  
Nein. Da bei dieser Option nicht alle Einstellungen abgerufen werden, sollte sie nicht verwendet werden. Verwenden Sie stattdessen den Assistenten zum Erstellen der Basiskonfiguration auf dem zweiten Server und verwenden Sie den Synchronisierungsregel-Editor, um PowerShell-Skripts zum Verschieben benutzerdefinierter Regeln zwischen Servern zu erstellen. Siehe dazu [Verschieben der benutzerdefinierten Konfiguration vom aktiven Server auf den Stagingserver](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Problembehandlung
**F: Wie erhalte ich Hilfe zu Azure AD Connect?**

[Durchsuchen der Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Suchen Sie in der Microsoft Knowledge Base (KB) nach technischen Lösungen für häufige Probleme im Zusammenhang mit der Unterstützung von Azure AD Connect.

[Microsoft Azure Active Directory-Foren](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Sie können in der Community nach technischen Fragen und Antworten suchen oder eine eigene Frage stellen, indem Sie [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)klicken.

[Azure AD Connect-Kundensupport](https://manage.windowsazure.com/?getsupport=true)

* Verwenden Sie diesen Link, um Support über das Azure-Portal zu erhalten.




<!--HONumber=Nov16_HO3-->


