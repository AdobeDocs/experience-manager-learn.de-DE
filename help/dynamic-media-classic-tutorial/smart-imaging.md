---
title: Intelligente Bildbearbeitung
description: Die intelligente Bildbearbeitung in Dynamic Media Classic verbessert die Leistung bei der Bildbereitstellung, indem das Bildformat und die Qualität basierend auf den Funktionen des Client-Browsers automatisch optimiert werden. Dies geschieht durch Nutzung der AI-Funktionen von Adobe Sensei und Arbeiten mit vorhandenen Bildvorgaben. Erfahren Sie mehr über die intelligente Bildbearbeitung und wie Sie damit durch schnelleres Laden von Seiten bessere Kundenerlebnisse bieten können.
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 6%

---

# Intelligente Bildbearbeitung {#smart-imaging}

Einer der wichtigsten Aspekte des Kundenerlebnisses auf Ihrer Website, Ihrer mobilen Site oder App ist die Seitenladezeit. Kunden verlassen häufig eine Site oder App, wenn das Laden einer Seite zu lange dauert. Bilder stellen den Großteil der Seitenladezeit dar. Die intelligente Bildbearbeitung in Dynamic Media Classic verbessert die Leistung bei der Bildbereitstellung, indem das Bildformat und die Qualität basierend auf den Funktionen des Client-Browsers automatisch optimiert werden. Dies geschieht durch Nutzung der AI-Funktionen von Adobe Sensei und Arbeiten mit vorhandenen Bildvorgaben. Die intelligente Bildbearbeitung reduziert die Bildgröße um 30 Prozent oder mehr - was zu schnelleren Seitenladevorgängen und besseren Kundenerlebnissen führt.

Die intelligente Bildbearbeitung profitiert auch von der zusätzlichen Leistungssteigerung durch die vollständige Integration mit dem erstklassigen Premium-Service von Adobe. Dieser Dienst ermittelt die optimale Internet-Route zwischen Servern, Netzwerken und Austauschpunkten mit niedrigster Latenz und/oder Paketverlustrate im Vergleich zur Standardroute im Internet.

Weitere Informationen [Intelligente Bildbearbeitung](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html).

## Vorteile der intelligenten Bildbearbeitung

Da Bilder den Großteil der Ladezeit einer Seite ausmachen, kann die Leistungsverbesserung durch die intelligente Bildbearbeitung grundlegende Auswirkungen auf geschäftliche KPIs haben, z. B. höhere Konversionsraten, Besuchszeiten pro Site und niedrigere Absprungraten pro Site.

![image](assets/smart-imaging/smart-imaging-1.png)

## Funktionsweise der intelligenten Bildbearbeitung

Wie bereits erwähnt, nutzt die intelligente Bildbearbeitung die AI-Funktionen von Adobe Sensei und arbeitet mit vorhandenen Bildvorgaben zusammen, um Bilder automatisch in optimale Bildformate der nächsten Generation wie WebP zu konvertieren und dabei die visuelle Wiedergabetreue zu wahren.

Weitere Informationen [Funktionsweise der intelligenten Bildbearbeitung](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), einschließlich Details wie unterstützte Bildformate (und was passiert, wenn Sie diese Formate nicht verwenden) und deren Auswirkungen auf vorhandene verwendete Bildvorgaben.

## Auswirkungen der intelligenten Bildbearbeitung

Wahrscheinlich befürchten Sie, dass Sie Änderungen an Ihren URLs, Bildvorgaben und Code auf Ihrer Site vornehmen müssen, um die intelligente Bildbearbeitung nutzen zu können. Wenn Sie die Voraussetzungen für die Verwendung der intelligenten Bildbearbeitung erfüllen und nur mit Bildern in den unterstützten JPEG- und PNG-Bildformaten arbeiten, müssen Sie keine Änderungen vornehmen.

Intelligente Bildbearbeitung funktioniert mit Bildern, die über HTTP, HTTPS und HTTP/2 bereitgestellt werden.

>[!NOTE]
>
>Wenn Sie zur intelligenten Bildbearbeitung wechseln, wird der Cache im CDN gelöscht. Der Cache im CDN wird in der Regel innerhalb von ein oder zwei Tagen erneut aufgebaut.

Die intelligente Bildbearbeitung ist in Ihrer bestehenden Dynamic Media Classic-Lizenz enthalten. Für diese Funktion fallen keine zusätzlichen Kosten an. Um davon profitieren zu können, müssen Sie zwei Voraussetzungen erfüllen: Sie verfügen über ein von Adoben gebündeltes CDN und eine dedizierte Domäne. Dann müssen Sie es für Ihr Konto aktivieren, da es nicht automatisch aktiviert ist.

Die Aktivierung der intelligenten Bildbearbeitung beginnt mit dem Versand einer Anfrage an den technischen Support durch |Erstellen eines Support-Falles| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/de/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). Der Support arbeitet mit Ihnen zusammen, um eine benutzerdefinierte Domäne einzurichten, die Sie mit der intelligenten Bildbearbeitung verbinden. Sie ändern einen Parameter im Zusammenhang mit der Zwischenspeicherung (Time To Live oder TTL) und die Unterstützung löscht den Cache. Sie können auch einen optionalen Staging-Schritt ausführen, bevor Sie ihn zur Produktion verschieben. Wenn die intelligente Bildbearbeitung aktiviert ist, liefern Sie Kunden kleinere Bilder in derselben Qualität wie gewünscht. Das bedeutet, dass sie schnellere Seitenladezeiten erleben - und all dies geschieht automatisch, weil Adobe Sensei dabei hilft, die effizienteste Größe zu wählen.

Nachdem Sie die intelligente Bildbearbeitung aktiviert haben, möchten Sie überprüfen, ob sie erwartungsgemäß funktioniert.

Sie haben wahrscheinlich zusätzliche Fragen zur intelligenten Bildbearbeitung. Wir haben eine Liste häufig gestellter Fragen mit Antworten zusammengestellt. Lesen Sie die [FAQs](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html).

## Zusätzliche Ressourcen

Beobachten Sie die [Dynamic Media Classic Optimize Page Performance Skill Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) On-Demand-Webinar , um mehr über die intelligente Bildbearbeitung zu erfahren.
