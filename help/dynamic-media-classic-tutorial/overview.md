---
title: Willkommen beim Tutorial zu Best Practices für Dynamic Media Classic
description: Dynamic Media Classic ist der zentrale Ort, an dem Kunden Rich-Media-Inhalte erstellen, erstellen und bereitstellen. Dieses Tutorial mit Best Practices wurde erstellt, um aktuellen und neuen Benutzern von Dynamic Media Classic zu helfen, besser zu verstehen, was sie mit dieser leistungsstarken Rich-Media-Lösung aus Adobe tun können. In diesem Teil des Tutorials erfahren Sie, was Dynamic Media Classic ist, und erhalten einen kurzen Überblick über seine Kernfunktionen und Benutzeroberfläche.
sub-product: dynamic-media
doc-type: tutorial
topics: best-practices, development, authoring, configuring
audience: all
activity: develop, use
feature: Dynamic Media Classic
topic: Content Management
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 1%

---


# Willkommen beim Tutorial zu Best Practices für Dynamic Media Classic

Dieses Handbuch soll aktuellen und neuen Benutzern von Dynamic Media Classic helfen, besser zu verstehen, was sie mit ihrer leistungsstarken Rich-Media-Lösung aus Adobe tun können. Wir tun dies durch:

- Einführung in Dynamic Media Classic, Beschreibung dessen, was es ist, und Überblick über seine Kernfunktionen und Benutzeroberfläche.
- Erläuterung des allgemeinen Workflows Erstellen, Verfassen und Bereitstellen , den Sie beim Arbeiten mit Assets in der Lösung befolgen werden.
- Erörterung wichtiger Elemente, die vor dem Einstieg und der Verwendung der Lösung eingerichtet werden müssen.
- Machen Sie sich mit der Verwendung verschiedener Kernfunktionen der Lösung vertraut.

Im gesamten Handbuch werden wir Beispiele, Tipps und Best Practices bereitstellen. Darüber hinaus werden wichtige Begriffe und Konzepte erläutert, mit denen Sie bei der Arbeit mit Dynamic Media Classic vertraut sein sollten. Wenn Sie für ein bestimmtes Thema verfügbar sind, werden wir Sie auf relevante Webinare, Blog-Beiträge und Online-Dokumentation verweisen.

Wir hoffen, dass Ihnen dieser Leitfaden die Informationen zur Verfügung stellt, die Sie benötigen, um Ihre Dynamic Media Classic-Lösung von großem Nutzen zu nutzen. Um die Navigation in den Kapiteln dieses Handbuchs zu vereinfachen, klicken Sie auf das Lesezeichen-Symbol auf der linken Seite des Handbuchs, um dessen Inhalt anzuzeigen.

## Überblick über Dynamic Media Classic

Dynamic Media Classic ist der zentrale Ort, an dem Kunden Rich-Media-Inhalte erstellen, erstellen und bereitstellen. Dynamic Media Classic ist eine integrierte Rich-Media-Management-, Publishing- und Serving-Umgebung. Rich-Media können für alle Marketing- und Verkaufskanäle bereitgestellt werden, einschließlich Web, Druckmaterial, E-Mail-Kampagnen, Webanwendungen, Desktops und Geräten.

Die Bildbereitstellung ist möglicherweise die am häufigsten verwendete Funktion von Dynamic Media Classic. Tatsächlich verwenden die meisten Kunden Dynamic Media Classic, um alle Bilder auf ihren Websites bereitzustellen, einschließlich Bilder für Zoom oder Rich Media. Es kann jedoch auch für viele andere Zwecke verwendet werden, einschließlich der Bereitstellung von Videos und der Verwendung von AI zur Optimierung der bereitgestellten Bilder.

## Hauptfunktionen von Dynamic Media Classic

In diesem Handbuch besprechen wir die folgenden Kernfunktionen von Dynamic Media Classic.

- **Dynamic Imaging.** Der Dachbegriff für Echtzeitbearbeitung, Formatierung und Größenanpassung sowie interaktives Zoomen und Schwenken; Farb- und Texturbeobachtung; 360-Grad-Rotation; Bildvorlagen; und Multimedia-Viewern.
- **Video.** Laden Sie endgültige Videos hoch, veröffentlichen Sie sie und laden Sie sie schrittweise in konfigurierbare Video-Viewer herunter.
- **Smart Imaging.** Technologie, die die AI-Funktionen von Adobe Sensei nutzt und mit vorhandenen &quot;Bildvorgaben&quot;zusammenarbeitet, um die Leistung bei der Bildbereitstellung zu verbessern, indem Bildformat, -größe und -qualität basierend auf den Funktionen des Client-Browsers automatisch optimiert werden.

Weitere Funktionen der Lösung finden Sie in der [Dokumentation für Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/intro/introduction.html).

## Die Benutzeroberfläche von Dynamic Media Classic

Die Hauptbenutzeroberfläche von Dynamic Media Classic besteht aus drei Hauptbereichen: die globale Navigationsleiste, die Asset-Bibliothek und das Durchsuchenbedienfeld/Build-Bedienfeld.

![image](assets/overview/overview-dmc-ui-ew.png)

_Dynamic Media Classic-Benutzeroberfläche_

**Globale Navigationsleiste.** Am oberen Bildschirmrand können Sie über die Schaltflächen auf dieser Leiste auf die wichtigsten Bereiche und Funktionen der Lösung zugreifen. Sie können damit beispielsweise auf Upload-Funktionen zugreifen, verschiedene Asset-Baubereiche (Bildset, Rotationsset usw.) öffnen, wichtige Aufgaben wie das Einrichten von Bildvorgaben und Viewer-Vorgaben durchführen und Assets veröffentlichen. Von hier aus können Sie auch Ihre Aufträge überwachen, aktuelle Aktivitäten anzeigen und aus verschiedenen Hilfeoptionen auswählen.

**Asset-Bibliothek.** Auf der linken Seite des Bildschirms befindet sich die Asset-Bibliothek, ein Bedienfeld, mit dem Sie Ihre Assets in von Ihnen erstellten Ordnern und Unterordnern organisieren. Am oberen Rand des Bedienfelds finden Sie Such- und Filter, die Ihnen beim Suchen nach Assets helfen. Mit der erweiterten Suche können Sie suchen, indem Sie mehrere Optionen als Kriterien für Ihre Suche angeben, einschließlich ausgeblendeter Metadatenfelder, die an dieses Asset angehängt sind. Unten im Bedienfeld können Sie gelöschte Elemente sehen, indem Sie auf das Papierkorbsymbol klicken. Zunächst beginnen Sie nicht mit Ordnern, mit Ausnahme des Ordners der obersten Ebene, der denselben Namen wie Ihr Kontoname hat.

>[!NOTE]
>
>Assets im Papierkorb werden sieben Tage nach dem Einfügen automatisch dauerhaft gelöscht, es sei denn, Sie stellen sie wieder her.

**Bedienfeld &quot;Durchsuchen/Erstellen&quot;.** Dies ist der Mittelpunkt der Benutzeroberfläche, in der Sie entweder Assets im Durchsuchen-Modus durchsuchen oder im Build-Modus als Arbeitsfläche zum Erstellen von Assets im Rahmen eines Workflows verwenden. Wenn Sie sich zum ersten Mal anmelden, wird das Bedienfeld &quot;Durchsuchen&quot;angezeigt. In der Mitte des Bildschirms befinden sich Miniaturansichten Ihrer Bilder in einer Rasteransicht. Sie können zu einer Listenansicht wechseln oder ein Asset auswählen und mithilfe der Detailansicht Details dazu anzeigen.

>[!IMPORTANT]
>
>Neben jeder Asset-ID befindet sich der Schalter **Zur Veröffentlichung markieren** . Wenn der Umschalter aktiviert ist (grün), zeigt dies an, dass das Asset zur Veröffentlichung markiert ist.

![image](assets/overview/overview-mark-for-publish.png)

>[!TIP]
>
>Aktivieren Sie im Dialogfeld &quot;Hochladen&quot;das Kontrollkästchen **Nach dem Hochladen veröffentlichen** , um Assets beim Hochladen automatisch zu veröffentlichen.

Erfahren Sie mehr über [Navigieren in der Benutzeroberfläche von Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/getting-started/navigation-basics.html).
