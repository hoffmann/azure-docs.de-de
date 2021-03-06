---
title: Wiederherstellen aus einer Sicherung des StorSimple Virtual Array
description: Erfahren Sie mehr dazu, wie Sie eine Sicherung des StorSimple Virtual Array wiederherstellen.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4d0deb8c-e3c7-4bc4-b89d-9881041960cb
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2801bc0fc645f9ed23733d1025f1cb42a5174022


---
# <a name="restore-from-a-backup-of-your-storsimple-virtual-array"></a>Wiederherstellen aus einer Sicherung des StorSimple Virtual Array
## <a name="overview"></a>Übersicht
Dieser Artikel bezieht sich auf das Microsoft Azure StorSimple Virtual Array (auch als „lokales virtuelles StorSimple-Gerät“ oder „virtuelles StorSimple-Gerät“ bezeichnet) mit der Version vom März 2016 (allgemeine Verfügbarkeit) oder höher. In diesem Artikel wird Schritt für Schritt beschrieben, wie Sie die Wiederherstellung aus einem Sicherungssatz Ihrer Freigaben oder Volumes auf dem StorSimple Virtual Array durchführen. Im Artikel wird auch ausführlich beschrieben, wie die Wiederherstellung auf Elementebene auf Ihrem StorSimple Virtual Array funktioniert, wenn dies als Dateiserver konfiguriert ist.

## <a name="restore-shares-from-a-backup-set"></a>Wiederherstellen von Freigaben aus einem Sicherungssatz
**Bevor Sie versuchen, die Freigaben wiederherzustellen, sollten Sie sich vergewissern, dass auf dem Gerät ausreichend Speicherplatz zum Durchführen dieses Vorgangs vorhanden ist.** Führen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/)die folgenden Schritte aus, um die Wiederherstellung aus einer Sicherung durchzuführen.

#### <a name="to-restore-a-share"></a>So führen Sie die Wiederherstellung von einer Freigabe durch
1. Navigieren Sie zum **Sicherungskatalog**. Filtern Sie nach dem entsprechenden Geräte- und Uhrzeitbereich, um nach Ihren Sicherungen zu suchen. Klicken Sie auf das Häkchensymbol ![](./media/storsimple-ova-restore/image1.png) , um die Abfrage durchzuführen.
2. Klicken Sie in der angezeigten Liste mit den Sicherungssätzen auf eine Sicherung, um sie auszuwählen. Erweitern Sie die Sicherung, um die darunter angeordneten Freigaben anzuzeigen. Klicken Sie auf eine Freigabe, um sie für die Wiederherstellung auszuwählen.
3. Klicken Sie unten auf der Seite auf **Als neu wiederherstellen**.
4. Der Assistent **Als neue Freigabe wiederherstellen** wird gestartet. Gehen Sie auf der Seite **Namen und Speicherort angeben** folgendermaßen vor:
   
   1. Überprüfen Sie den Namen des Quellgeräts. Dies sollte das Gerät mit der wiederherzustellenden Freigabe sein. Die Geräteauswahl ist deaktiviert. Um ein anderes Quellgerät auszuwählen, müssen Sie den Assistenten beenden und den Sicherungssatz erneut auswählen.
   2. Geben Sie einen Freigabenamen an. Der Freigabename kann 3 bis 127 Zeichen lang sein.
   3. Überprüfen Sie Größe, Typ und Berechtigungen der Freigabe, die Sie wiederherstellen möchten. Sie können die Freigabeeigenschaften mit dem Windows-Explorer ändern, nachdem die Wiederherstellung abgeschlossen ist.
   4. Klicken Sie auf das Häkchensymbol ![](./media/storsimple-ova-restore/image1.png).
      
      ![](./media/storsimple-ova-restore/image9.png)
5. Nachdem der Wiederherstellungsauftrag abgeschlossen ist, wird die Wiederherstellung gestartet, und es wird eine weitere Benachrichtigung angezeigt. Klicken Sie auf **Auftrag anzeigen**, um den Fortschritt der Wiederherstellung zu überwachen. Sie gelangen auf die Seite **Aufträge** .
6. Hier können Sie den Fortschritt des Wiederherstellungsauftrags verfolgen. Wenn die Wiederherstellung 100% erreicht hat, navigieren Sie zurück zur Seite **Freigaben** auf Ihrem Gerät.
7. Sie können nun die neu wiederhergestellte Freigabe in der Liste mit den Freigaben auf Ihrem Gerät anzeigen. Beachten Sie, dass bei der Wiederherstellung derselbe Freigabentyp verwendet wird. Eine mehrstufige Freigabe wird auch mehrstufig wiederhergestellt, und eine lokale Freigabe wird als lokale Freigabe wiederhergestellt.

Sie haben die Gerätekonfiguration nun abgeschlossen und gelernt, wie Sie eine Freigabe sichern und wiederherstellen. 

## <a name="restore-volumes-from-a-backup-set"></a>Wiederherstellen von Volumes aus einem Sicherungssatz
Führen Sie im klassischen Azure-Portal die folgenden Schritte aus, um die Wiederherstellung aus einer Sicherung durchzuführen. Beim Wiederherstellungsvorgang wird die Sicherung auf einem neuen Volume desselben virtuellen Geräts wiederhergestellt. Die Wiederherstellung auf einem anderen Gerät ist nicht möglich.

#### <a name="to-restore-a-volume"></a>So stellen Sie ein Volume wieder her
1. Navigieren Sie zum **Sicherungskatalog**. Filtern Sie nach dem entsprechenden Geräte- und Uhrzeitbereich, um nach Ihren Sicherungen zu suchen. Klicken Sie auf das Häkchensymbol ![](./media/storsimple-ova-restore/image1.png) , um die Abfrage durchzuführen.
2. Klicken Sie in der angezeigten Liste mit den Sicherungssätzen auf eine Sicherung, um sie auszuwählen. Erweitern Sie die Sicherung, um die darunter angeordneten Volumes anzuzeigen. Wählen Sie das Volume aus, das Sie wiederherstellen möchten. 
3. Klicken Sie unten auf der Seite auf **Als neu wiederherstellen**. Der Assistent **Als neues Volume wiederherstellen** wird gestartet.
4. Gehen Sie auf der Seite **Namen und Speicherort angeben** folgendermaßen vor:
   
   1. Überprüfen Sie den Namen des Quellgeräts. Dies sollte das Gerät mit dem wiederherzustellenden Volume sein. Die Geräteauswahl ist nicht verfügbar. Um ein anderes Quellgerät auszuwählen, müssen Sie den Assistenten beenden und den Sicherungssatz erneut auswählen.
   2. Geben Sie einen Volumenamen für das Volume ein, das als neues Volume wiederhergestellt wird. Der Volumename kann 3 bis 127 Zeichen lang sein.
   3. Klicken Sie auf das Pfeilsymbol.
      
      ![](./media/storsimple-ova-restore/image12.png)
5. Wählen Sie auf der Seite **Hosts angeben, die dieses Volume verwenden können** in der Dropdownliste die passenden ACRs aus.
   
   ![](./media/storsimple-ova-restore/image13.png)
6. Klicken Sie auf das Häkchensymbol ![](./media/storsimple-ova-restore/image1.png). Es wird ein Wiederherstellungsauftrag initiiert, und es wird die folgende Benachrichtigung mit dem Hinweis angezeigt, dass der Auftrag ausgeführt wird.
7. Nachdem der Wiederherstellungsauftrag abgeschlossen ist, wird die Wiederherstellung gestartet, und es wird eine weitere Benachrichtigung angezeigt. Klicken Sie auf **Auftrag anzeigen**, um den Fortschritt der Wiederherstellung zu überwachen. Sie gelangen auf die Seite **Aufträge** .
8. Hier können Sie den Fortschritt des Wiederherstellungsauftrags verfolgen. Navigieren Sie zurück zur Seite **Volumes** auf Ihrem Gerät.
9. Sie können nun das neu wiederhergestellte Volume in der Liste mit den Volumes auf Ihrem Gerät anzeigen. Beachten Sie, dass bei der Wiederherstellung derselbe Volumetyp verwendet wird. Ein mehrstufiges Volume wird auch als mehrstufiges Volume wiederhergestellt, und ein lokales Volume bleibt ein lokales Volume.
10. Sobald das Volume in der Liste der Volumes online angezeigt wird, ist das Volume für die Verwendung verfügbar.  Aktualisieren Sie auf dem iSCSI-Initiatorhost die Liste der Ziele im Fenster mit den iSCSI-Initiatoreigenschaften.  Ein neues Ziel, das den Namen des wiederhergestellten Volumes enthält, sollte in der Statusspalte als „inaktiv“ angezeigt werden.
11. Wählen Sie das Ziel aus, und klicken Sie auf **Verbinden**.   Wenn der Initiator mit dem Ziel verbunden ist, sollte sich der Status in **Verbunden**ändern. 
12. Im Fenster **Datenträgerverwaltung** werden die bereitgestellten Volumes wie in der folgenden Abbildung dargestellt angezeigt. Klicken Sie mit der rechten Maustaste auf das ermittelte Volume (klicken Sie auf den Datenträgernamen), und klicken Sie dann auf **Online**.

> [!IMPORTANT]
> Wenn Sie versuchen, ein Volume oder eine Freigabe aus einer Sicherung wiederherzustellen, und der Wiederherstellungsauftrag fehlschlägt, wird möglicherweise dennoch ein Zielvolume bzw. eine -freigabe im Portal erstellt. Es ist wichtig, dass Sie dieses Zielvolume bzw. die -freigabe im Portal löschen, um zukünftige Probleme mit diesem Element zu minimieren.
> 
> 

## <a name="item-level-recovery-ilr"></a>Wiederherstellung auf Elementebene
Mit dieser Version wird die Wiederherstellung auf Elementebene (Item-Level Recovery, ILR) auf einem virtuellen StorSimple-Gerät eingeführt, das als Dateiserver konfiguriert ist. Mit diesem Feature können Sie die Dateien und Ordner einer Cloudsicherung für alle Freigaben des StorSimple-Geräts präzise wiederherstellen. Benutzer können gelöschte Dateien aus kürzlich erfolgten Sicherungen per Self-Service wiederherstellen.

Jede Freigabe verfügt über einen Ordner *.backups* , der die letzten Sicherungen enthält. Der Benutzer kann zur gewünschten Sicherung navigieren, relevante Dateien und Ordner aus der Sicherung kopieren und diese dann wiederherstellen. Es ist dann nicht mehr erforderlich, sich wegen der Wiederherstellung von Dateien aus Sicherungen an den Administrator zu wenden.

1. Wenn Sie die Wiederherstellung auf Elementebene durchführen, können Sie die Sicherungen per Windows-Explorer anzeigen. Klicken Sie auf die jeweilige Freigabe, für die Sie sich die Sicherungen ansehen möchten. Sie sehen den Ordner *.backups* , der in der Freigabe erstellt wurde und alle Sicherungen enthält. Erweitern Sie den Ordner *.backups* , um die Sicherungen anzuzeigen. Im Ordner wird dann die Explosionsansicht der gesamten Sicherungshierarchie angezeigt. Diese Ansicht wird bei Bedarf erstellt, was normalerweise nur wenige Sekunden dauert.
   
   Auf diese Weise werden die letzten fünf Sicherungen angezeigt und können zur Wiederherstellung auf Elementebene verwendet werden. Zu den fünf letzten Sicherungen gehören sowohl die standardmäßig geplanten als auch die manuellen Sicherungen.

    -   **Geplante Sicherungen** weisen die Bezeichnung &lt;Gerätename&gt;DailySchedule-YYYYMMDD-HHMMSS-UTC auf.

    -   **Manuelle Sicherungen** weisen die Bezeichnung „Ad-hoc-YYYYMMDD-HHMMSS-UTC“ auf.

        ![](./media/storsimple-ova-restore/image14.png)

1. Identifizieren Sie die Sicherung mit der letzten Version der gelöschten Datei. Der Ordnername enthält zwar in allen obigen Fällen einen UTC-Zeitstempel, aber der Zeitpunkt der Ordnererstellung ist die eigentliche Geräteuhrzeit, zu der die Sicherung gestartet wurde. Verwenden Sie den Ordnerzeitstempel, um die Sicherungen zu finden und zu identifizieren.
2. Suchen Sie nach dem Ordner oder der Datei, den bzw. die Sie in der im vorherigen Schritt identifizierten Sicherung wiederherstellen möchten. Beachten Sie, dass Sie nur die Dateien oder Ordner anzeigen können, für die Sie Berechtigungen besitzen. Wenn Sie nicht auf bestimmte Dateien oder Ordner zugreifen können, müssen Sie sich an einen Freigabeadministrator wenden. Der Administrator kann Windows-Explorer verwenden, um die Freigabeberechtigungen zu bearbeiten und Ihnen den Zugriff auf die Datei oder den Ordner zu gewähren. Es ist eine empfohlene bewährte Methode, dass es sich beim Freigabeadministrator um eine Benutzergruppe handelt, nicht um einen einzelnen Benutzer.
3. Kopieren Sie die Datei bzw. den Ordner auf die entsprechende Freigabe auf dem StorSimple-Dateiserver.

![video_icon](./media/storsimple-ova-restore/video_icon.png) **Video verfügbar**

In diesem Video wird gezeigt, wie Sie Freigaben erstellen, Freigaben sichern und Daten auf einem StorSimple Virtual Array wiederherstellen.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Use-the-StorSimple-Virtual-Array/player]
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr darüber, wie Sie das [StorSimple Virtual Array mit der lokalen Webbenutzeroberfläche verwalten](storsimple-ova-web-ui-admin.md).




<!--HONumber=Nov16_HO3-->


