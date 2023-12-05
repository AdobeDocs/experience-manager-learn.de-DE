---
title: Intelligente Bildbearbeitung
description: Die intelligente Bildbearbeitung in Dynamic Media Classic verbessert die Leistung bei der Bildbereitstellung, indem das Bildformat und die Qualität basierend auf den Funktionen des Client-Browsers automatisch optimiert werden. Dies geschieht durch Nutzung der KI-Funktionen von Adobe Sensei und Arbeiten mit vorhandenen Bildvorgaben. Erfahren Sie mehr über die intelligente Bildbearbeitung und darüber, wie Sie damit durch schnelleres Laden von Seiten bessere Kundenerlebnisse bieten können.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
duration: 166
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 100%

---

# Intelligente Bildbearbeitung {#smart-imaging}

Einer der wichtigsten Aspekte des Kundenerlebnisses auf Ihrer Website, Ihrer mobilen Site oder App ist die Seitenladezeit. Kundinnen und Kunden verlassen häufig eine Site oder App, wenn das Laden einer Seite zu lange dauert. Bilder verursachen den Großteil der Seitenladezeit. Die intelligente Bildbearbeitung in Dynamic Media Classic verbessert die Leistung bei der Bildbereitstellung, indem das Bildformat und die Qualität basierend auf den Funktionen des Client-Browsers automatisch optimiert werden. Dies geschieht durch Nutzung der KI-Funktionen von Adobe Sensei und Arbeiten mit vorhandenen Bildvorgaben. Die intelligente Bildbearbeitung reduziert die Bildgröße um 30 Prozent oder mehr, was zu schnelleren Seitenladevorgängen und besseren Kundenerlebnissen führt.

Die intelligente Bildbearbeitung profitiert auch von der zusätzlichen Leistungssteigerung durch die vollständige Integration mit dem marktführendem Premium-Dienst von Adobe. Dieser Dienst ermittelt die optimale Internet-Route zwischen Servern, Netzwerken und Austauschpunkten mit niedrigster Latenz und/oder Paketverlustrate im Vergleich zur Standardroute im Internet.

Weitere Informationen über die [Intelligente Bildbearbeitung](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=de).

## Vorteile der intelligenten Bildbearbeitung

Da Bilder einen Großteil der Seitenladezeit ausmachen, kann die Leistungssteigerung der intelligenten Bildbearbeitung einen enormen Einfluss auf geschäftsbezogene KPIs wie höhere Konversionsraten, auf der Site verbrachte Zeit und niedrigere Website-Absprungraten haben.

![Bild](assets/smart-imaging/smart-imaging-1.png)

## Funktionsweise der intelligenten Bildbearbeitung

Wie bereits erwähnt, nutzt die intelligente Bildbearbeitung die KI-Funktionen von Adobe Sensei und arbeitet mit vorhandenen Bildvorgaben zusammen, um Bilder automatisch in optimale Bildformate der nächsten Generation wie WebP zu konvertieren und dabei die visuelle Wiedergabetreue zu wahren.

Informieren Sie sich hier über die [Funktionsweise der intelligenten Bildbearbeitung](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=de#how-does-smart-imaging-work), einschließlich Details wie unterstützte Bildformate (und was passiert, wenn Sie diese Formate nicht verwenden) und die Auswirkung auf vorhandene Bildvorgaben, die verwendet werden.

## Auswirkungen der intelligenten Bildbearbeitung

Wahrscheinlich befürchten Sie, dass Sie Änderungen an Ihren URLs, Bildvorgaben und Code auf Ihrer Site vornehmen müssen, um die intelligente Bildbearbeitung nutzen zu können. Wenn Sie die Voraussetzungen für die Verwendung der intelligenten Bildbearbeitung erfüllen und nur mit Bildern in den unterstützten JPEG- und PNG-Bildformaten arbeiten, müssen Sie jedoch keine Änderungen vornehmen.

Intelligente Bildbearbeitung funktioniert mit Bildern, die über HTTP, HTTPS und HTTP/2 bereitgestellt werden.

>[!NOTE]
>
>Wenn Sie zur intelligenten Bildbearbeitung wechseln, wird der Cache im CDN gelöscht. Der Cache im CDN wird in der Regel innerhalb von ein oder zwei Tagen erneut aufgeladen.

Die intelligente Bildbearbeitung ist in Ihrer bestehenden Dynamic Media Classic-Lizenz enthalten. Für diese Funktion fallen keine zusätzlichen Kosten an. Um die Vorteile zu nutzen, müssen Sie zwei Voraussetzungen erfüllen: Sie müssen über ein von Adobe gebündeltes CDN und eine eigene Domain verfügen. Dann müssen Sie es für Ihr Konto aktivieren, da es nicht automatisch aktiviert ist.

Die Aktivierung der intelligenten Bildbearbeitung beginnt mit dem Versand einer Anfrage an den technischen Support durch |Erstellen eines Support-Falles| [https://helpx.adobe.com/de/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/de/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). Der Support arbeitet mit Ihnen zusammen, um eine benutzerdefinierte Domain einzurichten, die Sie mit der intelligenten Bildbearbeitung verbinden. Sie ändern einen Parameter im Zusammenhang mit der Zwischenspeicherung (Time To Live oder TTL) und der Support löscht den Cache. Sie können auch einen optionalen Staging-Schritt ausführen, bevor Sie in die Produktion übertragen. Wenn die intelligente Bildbearbeitung aktiviert ist, liefern Sie Kundinnen und Kunden kleinere Bilder in derselben Qualität wie gewünscht. Das bedeutet, dass sie schnellere Seitenladezeiten erleben – und all dies geschieht automatisch, weil Adobe Sensei dabei hilft, die effizienteste Größe zu wählen.

Nachdem Sie die intelligente Bildbearbeitung aktiviert haben, möchten Sie sicher überprüfen, ob sie erwartungsgemäß funktioniert.

Wahrscheinlich haben Sie weitere Fragen zur intelligenten Bildbearbeitung. Wir haben eine Liste häufig gestellter Fragen mit Antworten zusammengestellt. Lesen Sie die [Häufig gestellte Fragen](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=de).

## Zusätzliche Ressourcen

Sehen Sie sich das On-Demand-Webinar [Dynamic Media Classic Optimizing Page Performance Skill Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) an, um mehr über die intelligente Bildbearbeitung zu erfahren.
