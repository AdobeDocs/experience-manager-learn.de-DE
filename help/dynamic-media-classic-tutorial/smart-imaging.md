---
title: Smart Imaging
description: Die intelligente Bildbearbeitung in Dynamic Media Classic verbessert die Leistung bei der Bildbereitstellung, indem das Bildformat und die Qualität basierend auf den Funktionen des Client-Browsers automatisch optimiert werden. Dies geschieht durch Nutzung der AI-Funktionen von Adobe Sensei und Arbeiten mit vorhandenen Bildvorgaben. Erfahren Sie mehr über die intelligente Bildbearbeitung und wie Sie damit durch schnelleres Laden von Seiten bessere Kundenerlebnisse bieten können.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 9%

---


# Intelligente Bildbearbeitung {#smart-imaging}

Einer der wichtigsten Aspekte des Kundenerlebnisses auf Ihrer Website, Ihrer mobilen Site oder App ist die Seitenladezeit. Kunden verlassen häufig eine Site oder App, wenn das Laden einer Seite zu lange dauert. Bilder stellen den Großteil der Seitenladezeit dar. Die intelligente Bildbearbeitung in Dynamic Media Classic verbessert die Leistung bei der Bildbereitstellung, indem das Bildformat und die Qualität basierend auf den Funktionen des Client-Browsers automatisch optimiert werden. Dies geschieht durch Nutzung der AI-Funktionen von Adobe Sensei und Arbeiten mit vorhandenen Bildvorgaben. Die intelligente Bildbearbeitung reduziert die Bildgröße um 30 Prozent oder mehr - was zu schnelleren Seitenladevorgängen und besseren Kundenerlebnissen führt.

Die intelligente Bildbearbeitung profitiert auch von der zusätzlichen Leistungssteigerung durch die vollständige Integration mit dem erstklassigen Premium-Service von Adobe. Dieser Dienst ermittelt die optimale Internet-Route zwischen Servern, Netzwerken und Austauschpunkten mit niedrigster Latenz und/oder Paketverlustrate im Vergleich zur Standardroute im Internet.

Erfahren Sie mehr über [Intelligente Bildbearbeitung](https://docs.adobe.com/content/help/de-DE/experience-manager-64/assets/dynamic/imaging-faq.html).

## Vorteile der intelligenten Bildbearbeitung

Da Bilder den Großteil der Ladezeit einer Seite ausmachen, kann die Leistungsverbesserung durch die intelligente Bildbearbeitung grundlegende Auswirkungen auf geschäftliche KPIs haben, z. B. höhere Konversionsraten, Besuchszeiten pro Site und niedrigere Absprungraten pro Site.

![image](assets/smart-imaging/smart-imaging-1.png)

## Funktionsweise der intelligenten Bildbearbeitung

Wie bereits erwähnt, nutzt die intelligente Bildbearbeitung die AI-Funktionen von Adobe Sensei und arbeitet mit vorhandenen Bildvorgaben zusammen, um Bilder automatisch in optimale Bildformate der nächsten Generation wie WebP zu konvertieren und dabei die visuelle Wiedergabetreue zu wahren.

Erfahren Sie mehr über [Funktionsweise der intelligenten Bildbearbeitung](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), einschließlich Details wie unterstützte Bildformate (und was passiert, wenn Sie diese Formate nicht verwenden) und deren Auswirkungen auf vorhandene verwendete Bildvorgaben.

## Auswirkungen der intelligenten Bildbearbeitung

Wahrscheinlich befürchten Sie, dass Sie Änderungen an Ihren URLs, Bildvorgaben und Code auf Ihrer Site vornehmen müssen, um die intelligente Bildbearbeitung nutzen zu können. Wenn Sie die Voraussetzungen für die Verwendung der intelligenten Bildbearbeitung erfüllen und nur mit Bildern in den unterstützten JPEG- und PNG-Bildformaten arbeiten, müssen Sie keine Änderungen vornehmen.

Intelligente Bildbearbeitung funktioniert mit Bildern, die über HTTP, HTTPS und HTTP/2 bereitgestellt werden.

>[!NOTE]
>
>Wenn Sie zur intelligenten Bildbearbeitung wechseln, wird der Cache im CDN gelöscht. Der Cache im CDN wird in der Regel innerhalb von ein oder zwei Tagen erneut aufgebaut.

Die intelligente Bildbearbeitung ist in Ihrer bestehenden Lizenz für Dynamic Media Classic enthalten. Für diese Funktion fallen keine zusätzlichen Kosten an. Um davon profitieren zu können, müssen Sie zwei Voraussetzungen erfüllen: Sie verfügen über ein von Adoben gebündeltes CDN und eine dedizierte Domäne. Dann müssen Sie es für Ihr Konto aktivieren, da es nicht automatisch aktiviert ist.

Die Aktivierung der intelligenten Bildbearbeitung beginnt mit dem Versand einer Anfrage an den technischen Support durch |Erstellen eines Support-Falles| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/de/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). Der Support arbeitet mit Ihnen zusammen, um eine benutzerdefinierte Domäne einzurichten, die Sie mit der intelligenten Bildbearbeitung verbinden. Sie ändern einen Parameter im Zusammenhang mit der Zwischenspeicherung (Time To Live oder TTL) und die Unterstützung löscht den Cache. Sie können auch einen optionalen Staging-Schritt ausführen, bevor Sie ihn zur Produktion verschieben. Wenn die intelligente Bildbearbeitung aktiviert ist, liefern Sie Kunden kleinere Bilder in derselben Qualität wie gewünscht. Das bedeutet, dass sie schnellere Seitenladezeiten erleben - und all dies geschieht automatisch, weil Adobe Sensei dabei hilft, die effizienteste Größe zu wählen.

Nachdem Sie die intelligente Bildbearbeitung aktiviert haben, möchten Sie überprüfen, ob sie erwartungsgemäß funktioniert.

Sie haben wahrscheinlich zusätzliche Fragen zur intelligenten Bildbearbeitung. Wir haben eine Liste häufig gestellter Fragen mit Antworten zusammengestellt. Lesen Sie die [FAQs](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html).

## Zusätzliche Ressourcen

Sehen Sie sich das On-Demand-Webinar [Dynamic Media Classic Optimizing Page Performance Skill Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) an, um mehr über die intelligente Bildbearbeitung zu erfahren.
