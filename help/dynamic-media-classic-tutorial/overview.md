---
title: Tutorial zu Best Practices für Dynamic Media Classic
description: Dynamic Media Classic ist ein zentraler Ort, an dem Kundinnen und Kunden Rich-Media-Inhalte erstellen, verfassen und bereitstellen können. Dieses Tutorial mit Best Practices soll bestehenden und neuen Benutzenden von Dynamic Media Classic vor Augen führen, wie sie diese leistungsstarke Rich-Media-Lösung von Adobe nutzen können. In diesem Teil des Tutorials erfahren Sie, was Dynamic Media Classic ist, und erhalten einen kurzen Überblick über Kernfunktionen und die Benutzeroberfläche.
doc-type: tutorial
audience: all
activity: develop, use
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
exl-id: 975b85af-ca6a-419e-ab2a-6e1781bfee4a
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: ht
source-wordcount: '885'
ht-degree: 100%

---

# Tutorial zu Best Practices für Dynamic Media Classic

Dieses Handbuch soll bestehenden und neuen Benutzenden von Dynamic Media Classic vor Augen führen, wie sie diese leistungsstarke Rich-Media-Lösung von Adobe nutzen können. Was Sie erwartet:

- Einführung in Dynamic Media Classic mit einer Beschreibung der Lösung und einem Überblick über die Kernfunktionen und die Benutzeroberfläche
- Erläuterung des allgemeinen Workflows zum Erstellen, Verfassen und Bereitstellen von Assets in der Lösung
- Erläuterung der Elemente, die vor der Verwendung der Lösung eingerichtet werden müssen
- Umfassende Erläuterung der Verwendung verschiedener Kernfunktionen der Lösung

Im gesamten Handbuch wird auf Beispiele, Tipps und Best Practices verwiesen. Wir erläutern auch wichtige Begriffe und Konzepte, mit denen Sie bei der Arbeit mit Dynamic Media Classic vertraut sein sollten. Sofern für ein bestimmtes Thema verfügbar, machen wir Sie auf relevante Webinare, Blogposts und Online-Dokumentation aufmerksam.

Wir hoffen, dass Ihnen dieses Handbuch die Informationen zur Verfügung stellt, die Sie benötigen, um den enormen Wert Ihrer Dynamic Media Classic-Lösung für sich zu nutzen. Für eine einfachere Navigation in den Kapiteln dieses Handbuchs klicken Sie auf das Lesezeichen-Symbol auf der linken Seite des Handbuchs, um den zugehörigen Inhalt anzuzeigen.

## Überblick über Dynamic Media Classic

Dynamic Media Classic ist ein zentraler Ort, an dem Kundinnen und Kunden Rich-Media-Inhalte erstellen, verfassen und bereitstellen können. Dynamic Media Classic ist eine integrierte Umgebung zum Verwalten, Veröffentlichen und Bereitstellen von Rich-Media-Inhalten. Rich Media können für alle Marketing- und Verkaufskanäle bereitgestellt werden, einschließlich Web, Druckmaterialien, E-Mail-Kampagnen, Web-Anwendungen, Desktops und Geräten.

Die Bildbereitstellung ist wahrscheinlich die am häufigsten verwendete Funktion von Dynamic Media Classic. Tatsächlich verwenden die meisten Kundinnen und Kunden Dynamic Media Classic, um Bilder auf ihren Websites bereitzustellen, darunter auch Bilder für Zoom oder Rich Media. Die Lösung kann jedoch auch für viele andere Zwecke verwendet werden, einschließlich der Bereitstellung von Videos und der Verwendung von KI zur Optimierung der bereitgestellten Bilder.

## Kernfunktionen von Dynamic Media Classic

In diesem Handbuch werden die folgenden Kernfunktionen von Dynamic Media Classic behandelt.

- **Dynamic Imaging.** Der Überbegriff für die Bearbeitung, Formatierung und Größenanpassung in Echtzeit sowie interaktives Zoomen und Schwenken, Farb- und Texturmuster, 360-Grad-Rotation, Bildvorlagen und Multimedia-Viewer.
- **Video.** Laden Sie fertige Videos hoch, veröffentlichen Sie sie und laden Sie sie progressiv in konfigurierbare Video-Viewer herunter.
- **Intelligente Bildbearbeitung.** Diese Technologie nutzt die KI-Funktionen von Adobe Sensei und vorhandene „Bildvorgaben“, um die Bereitstellung von Bildern zu verbessern, indem Bildformat, Größe und Qualität basierend auf den Funktionen des Client-Browsers automatisch optimiert werden.

Weitere Informationen zu den zusätzlichen Funktionen der Lösung finden Sie in der [Dokumentation für Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/intro/introduction.html?lang=de).

## Die Dynamic Media Classic-Benutzeroberfläche

Die Hauptbenutzeroberfläche von Dynamic Media Classic besteht aus drei Bereichen: der globalen Navigationsleiste, der Asset-Bibliothek und dem Durchsuchen-/Erstellen-Panel.

![Bild](assets/overview/overview-dmc-ui-ew.png)

_Dynamic Media Classic-Benutzeroberfläche_

**Globale Navigationsleiste.** Diese Leiste befindet sich am oberen Bildschirmrand. Über ihre Schaltflächen können Sie auf die wichtigsten Bereiche und Funktionen der Lösung zugreifen. Sie können damit beispielsweise auf Upload-Funktionen zugreifen, verschiedene Bereich für die Asset-Erstellung öffnen (Bild-Set, Rotations-Set usw.) und wichtige Aufgaben durchführen wie das Einrichten von Bild- und Viewer-Voreinstellungen und das Veröffentlichen von Assets. Von hier aus können Sie auch Vorgänge überwachen, kürzlich durchgeführte Aktivitäten anzeigen und aus verschiedenen Hilfeoptionen auswählen.

**Asset-Bibliothek.** Auf der linken Seite des Bildschirms befindet sich die Asset-Bibliothek. In diesem Panel können Sie Ihre Assets in von Ihnen erstellten Ordnern und Unterordnern organisieren. Am oberen Rand des Panels gibt es die Suchfunktion und Filter, die Ihnen bei der Suche nach Assets helfen. Die erweiterte Suche können Sie verwenden, indem Sie mehrere Optionen als Kriterien für Ihre Suche angeben, einschließlich ausgeblendeter Metadatenfelder, die mit diesem Asset verknüpft sind. Unten im Panel können Sie gelöschte Elemente sehen, indem Sie auf das Papierkorbsymbol klicken. Zunächst verwenden Sie keine Ordner – mit Ausnahme des Ordners der obersten Ebene, der denselben Namen wie Ihr Konto trägt.

>[!NOTE]
>
>Assets im Papierkorb werden sieben Tage nach dem Verschieben dorthin automatisch dauerhaft gelöscht, es sei denn, Sie entfernen sie wieder aus dem Papierkorb.

**Durchsuchen-/Erstellen-Panel.** Dieses Panel befindet sich in der Mitte der Benutzeroberfläche. Dort können Sie entweder Assets im Durchsuchen-Modus suchen oder im Erstellen-Modus für einen Workflow erstellen. Wenn Sie sich zum ersten Mal anmelden, wird das Durchsuchen-Panel angezeigt. In der Mitte des Bildschirms befinden sich Miniaturansichten Ihrer Bilder in einer Rasteransicht. Sie können zu einer Listenansicht wechseln oder ein Asset auswählen und mithilfe der Detailansicht Details dazu anzeigen.

>[!IMPORTANT]
>
>Neben jeder Asset-ID befindet sich der Umschalter **Zur Veröffentlichung markieren**. Wenn der Umschalter aktiviert (grün) ist, zeigt dies an, dass das Asset zur Veröffentlichung markiert ist.

![Bild](assets/overview/overview-mark-for-publish.png)

>[!TIP]
>
>Wählen Sie im Hochladedialogfeld das Kontrollkästchen **Nach dem Hochladen veröffentlichen**, um Assets beim Hochladen automatisch zu veröffentlichen.

Hier finden Sie weitere Informationen zum [Navigieren in der Benutzeroberfläche von Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/navigation-basics.html?lang=de).
