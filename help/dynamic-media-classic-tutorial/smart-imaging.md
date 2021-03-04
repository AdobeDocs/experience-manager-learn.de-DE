---
title: Smart Imaging
description: Smart Imaging in Dynamic Media Classic steigert die Leistung von Image Versand durch die automatische Optimierung von Bildformat und -qualität auf der Grundlage der Browserfunktionen des Kunden. Dies geschieht durch die Nutzung der AI-Funktionen von Adobe Sensei und die Arbeit mit vorhandenen Bildvorgaben. Erfahren Sie mehr über Smart Imaging und wie Sie damit bessere Kundenerlebnisse durch schnelleres Laden von Seiten erzielen können.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Content Management
role: Geschäftspraktiker
level: Anfänger
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 8%

---


# Intelligente Bildbearbeitung {#smart-imaging}

Einer der wichtigsten Aspekte des Kundenerlebnisses auf Ihrer Website, mobilen Site oder App ist die Seitenladezeit. Kunden verlassen oft eine Site oder App, wenn das Laden einer Seite zu lange dauert. Bilder stellen den Großteil der Seitenladezeit dar. Smart Imaging in Dynamic Media Classic steigert die Leistung von Image Versand durch die automatische Optimierung von Bildformat und -qualität auf der Grundlage der Browserfunktionen des Kunden. Dies geschieht durch die Nutzung der AI-Funktionen von Adobe Sensei und die Arbeit mit vorhandenen Bildvorgaben. Smart Imaging reduziert Bildgrößen um 30 Prozent oder mehr — das zu schnelleren Seitenladevorgängen und besseren Kundenerlebnissen führt.

Smart Imaging profitiert auch von der zusätzlichen Leistungssteigerung, die durch die vollständige Integration mit dem erstklassigen Premium-Service der Adobe entsteht. Dieser Dienst ermittelt die optimale Internet-Route zwischen Servern, Netzwerken und Austauschpunkten mit niedrigster Latenz und/oder Paketverlustrate im Vergleich zur Standardroute im Internet.

Erfahren Sie mehr über [Smart Imaging](https://docs.adobe.com/content/help/de-DE/experience-manager-64/assets/dynamic/imaging-faq.html).

## Vorteile von Smart Imaging

Da Bilder den Großteil der Ladezeit einer Seite ausmachen, kann die Leistungsverbesserung durch Smart Imaging grundlegende Auswirkungen auf Geschäfts-KPIs haben, z. B. eine höhere Umrechnung, Besuchszeit vor Ort und eine niedrigere Absprungrate der Site.

![image](assets/smart-imaging/smart-imaging-1.png)

## Funktionsweise von Smart Imaging

Wie bereits erwähnt, nutzt Smart Imaging die AI-Funktionen von Adobe Sensei und arbeitet mit vorhandenen Bildvorgaben zusammen, um Bilder automatisch in optimale Bildformate der nächsten Generation wie WebP zu konvertieren und dabei die visuelle Genauigkeit zu erhalten.

Erfahren Sie mehr über [Funktionsweise von Smart Imaging](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), einschließlich Details wie unterstützte Bildformate (und was passiert, wenn Sie diese Formate nicht verwenden) und deren Auswirkungen auf vorhandene Bildvorgaben, die verwendet werden.

## Auswirkungen von Smart Imaging

Sie befürchten, dass Sie Änderungen an URLs, Bildvorgaben und Code auf Ihrer Site vornehmen müssen, um Smart Imaging nutzen zu können. Wenn Sie die Voraussetzungen für die Verwendung von Smart Imaging erfüllen und nur mit Bildern in den unterstützten JPEG- und PNG-Bildformaten arbeiten, müssen Sie keine Änderungen vornehmen.

Die intelligente Bildbearbeitung funktioniert mit Bildern, die über HTTP, HTTPS und HTTP/2 bereitgestellt werden.

>[!NOTE]
>
>Wenn Sie zu Smart Imaging wechseln, wird Ihr Cache im CDN gelöscht. Der Cache im CDN wird in der Regel innerhalb von ein oder zwei Tagen wieder aufgebaut.

Smart Imaging ist in Ihrer bestehenden Lizenz von Dynamic Media Classic enthalten. Für diese Funktion fallen keine zusätzlichen Kosten an. Um diese Vorteile zu nutzen, müssen Sie zwei Anforderungen erfüllen: haben ein CDN mit Adobe und eine dedizierte Domäne. Dann müssen Sie es für Ihr Konto aktivieren, da es nicht automatisch aktiviert ist.

Aktivieren von Smart Imaging-Beginn, wenn Sie eine Anfrage an den technischen Support senden, indem Sie |Erstellen eines Supportfalls| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/de/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). Der Support arbeitet mit Ihnen zusammen, um eine benutzerdefinierte Domäne einzurichten, die Sie mit Smart Imaging verbinden. Sie ändern einen Parameter im Zusammenhang mit der Zwischenspeicherung (Time To Live oder TTL) und die Unterstützung löscht den Cache. Sie können auch einen optionalen Staging-Schritt ausführen, bevor Sie zur Produktion wechseln. Wenn Smart Imaging dann aktiviert ist, liefern Sie den Kunden kleinere Bilder, aber mit der gleichen Qualität, die sie angefordert haben. Das bedeutet, dass die Seitenladezeiten schneller ablaufen — Und das alles geschieht automatisch, weil Adobe Sensei dabei hilft, die effizienteste Größe zu wählen.

Sobald Sie Smart Imaging aktiviert haben, sollten Sie überprüfen, ob es erwartungsgemäß funktioniert.

Sie haben wahrscheinlich noch weitere Fragen zu Smart Imaging. Wir haben eine Liste häufig gestellter Fragen mit Antworten zusammengestellt. Lesen Sie die [FAQs](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html).

## Zusätzliche Ressourcen

Sehen Sie sich das Webinar [Dynamic Media Classic Optimizing Page Performance Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) an, um mehr über Smart Imaging zu erfahren.
