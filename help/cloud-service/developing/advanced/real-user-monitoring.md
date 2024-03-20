---
title: Real User Monitoring (RUM)
description: Erfahren Sie mehr über die Echtzeit-Benutzerüberwachung (RUM) auf AEM as a Cloud Service Website.
version: Cloud Service
feature: Operations
topic: Performance
role: Admin, Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 0
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-03-18T00:00:00Z
source-git-commit: 7c80bb25b79a77c4a0bb2bbedf8a7c338177b857
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---


# Real User Monitoring (RUM)

Erfahren Sie mehr über die Echtzeit-Benutzerüberwachung (RUM) auf AEM as a Cloud Service Website. Erfahren Sie, wie Sie RUM aktivieren, welche Daten erfasst werden und wie Sie RUM-Daten verwenden, um das Benutzererlebnis auf Ihrer Website zu optimieren.

## Übersicht

Real User Monitoring (RUM) ist eine Methode, mit der _Benutzerinteraktionen und -erlebnisse erfassen, messen und analysieren_ mit einer Website in Echtzeit. Sie bietet Einblicke in die Interaktion der Site-Besucher mit Ihrer Website, einschließlich Verhalten, Leistung und Gesamterlebnis. Dies wird erreicht, indem ein kleiner JavaScript-Code in die Seiten der Site eingefügt wird.

Mit JavaScript-Code erfasst RUM Daten direkt aus dem Browser des Benutzers bei der Interaktion mit Ihrer Website. Diese Daten können verwendet werden, um Leistungsprobleme zu identifizieren und zu diagnostizieren, das Benutzererlebnis zu optimieren und die Geschäftsergebnisse zu verbessern.

Die RUM-Funktion in AEM as a Cloud Service bietet einen umfassenden Überblick über das Benutzererlebnis auf Ihrer Website. Sie erfasst die folgenden Schlüsselmetriken für jede vom Benutzer besuchte Seite (URL):

- [Größte inhaltsreiche Farbe (LCP)](https://web.dev/articles/lcp) - die Ladeleistung misst.
- [Kumulativer Layoutwechsel (CLS)](https://web.dev/articles/cls) - misst die visuelle Stabilität.
- [Interaktion mit der nächsten Farbe (INP)](https://web.dev/articles/inp) - Maßnahmen zur Interaktivität.
- Seitenansichten - misst, wie oft eine Seite angezeigt wird.

Außerdem werden die 404-Fehler- und Seitenansichtsdiagramme für die Website erfasst.

Die Metriken LCP, CLS und INP sind Teil der [Core Web Vitals](https://web.dev/articles/vitals) Hierbei handelt es sich um eine Reihe von Metriken zur Geschwindigkeit, Reaktionsfähigkeit und visuellen Stabilität einer Website. Diese Metriken werden von Google verwendet, um das Benutzererlebnis auf einer Website zu messen, und sind für den Suchmaschinenranking wichtig.

## RUM aktivieren

Informationen zum Aktivieren von RUM für Ihre AEMCS-Website finden Sie unter [Einrichten des Echtzeit-Benutzerüberwachungsdienstes](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#how-to-set-up-the-rum-service).

Die wichtigsten Details von RUM in AEMCS sind:

- Das RUM gilt nur für den Veröffentlichungsdienst von AEMCS, d. h. JavaScript-Code wird nur in die Veröffentlichungsumgebung eingefügt.
- Die `com.adobe.granite.webvitals.WebVitalsConfig` Die OSGi-Konfiguration steuert die Ein- und Ausschlusspfade. Dies sind Repository-Pfade und keine URL-Pfade.
- Standardmäßig `/content` path ist enthalten.
- Um Pfade auszuschließen, fügen Sie die `AEM_WEBVITALS_EXCLUDE` Cloud Manager-Umgebungsvariable, siehe [Hinzufügen von Umgebungsvariablen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#add-variables). Die Pfade werden durch Kommas getrennt.
- Der OOTB-Code ist für das Einfügen des JavaScript-Codes in die Seiten verantwortlich.

### Überprüfung

Um zu überprüfen, ob RUM für Ihre Website aktiviert ist, rufen Sie die HTML-Quelle der veröffentlichten Seite auf und suchen Sie nach den folgenden Skriptblöcken:

```html
...

<!-- Added before the closing </head> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('top');
    window.addEventListener('load', () => sampleRUM('load'));
    document.addEventListener('click', () => sampleRUM('click'));
</script>

...

<!-- Added before the closing </body> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('lazy');
    sampleRUM('cwv');
</script>
```

## RUM-Datenerfassung

- Die RUM-Daten werden mithilfe von `sampleRUM()` -Funktion, indem Sie den Checkpoint-Namen übergeben. Im obigen Beispiel sind die Checkpoints `top`, `load`, `click`, `lazy`, und `cwv`.
- Ein Checkpoint ist ein benanntes Ereignis in der Sequenz, in der die Seite geladen und damit interagiert wird.

Siehe auch [Real User Monitoring Service und Datenschutz](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#rum-service-and-privacy) und [Welche Daten werden erfasst?](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#what-data-is-being-collected) für weitere Details.

## RUM-Datenansicht

Zum Anzeigen der benötigten RUM-Daten `domainkey`, stellt Adobe sie als Teil des RUM-Setups bereit. Das RUM-Dashboard ist verfügbar unter [https://data.aem.live/](https://data.aem.live/) und Sie können darauf über den Domain-Schlüssel und die URL zugreifen.

Beispielsweise zeigt der folgende Screenshot das RUM-Dashboard für die AEM WKND-Website.

![RUM-Dashboard](./assets/rum/RUM-Dashboard-WKND.png)

Das RUM-Dashboard bietet die folgenden wichtigen Einblicke:

- **Leistungsmetriken** - LCP, CLS, INP und Seitenansichten.
- **Fehlermetriken** - 404 Fehler.
- **Diagramme für Seitenansichten** - Anzahl der Seitenansichten im Zeitverlauf.

## Verwenden von RUM-Daten

Mithilfe der oben genannten Einblicke können Sie das Benutzererlebnis auf Ihrer Website optimieren. Zum Beispiel:

- Reduzieren Sie LCP, CLS und INP, um die Seitenladeleistung und Interaktivität zu verbessern. Siehe [Verbesserung des LCP](https://web.dev/articles/lcp#improve-lcp), [Verbesserung von CLS](https://web.dev/articles/cls#improve-cls) und [Verbessern des INP](https://web.dev/articles/inp#improve-inp)für weitere Details.
- Beheben Sie die 404-Fehler, um das Benutzererlebnis zu verbessern.
- Um das Benutzerverhalten zu verstehen und den Inhalt zu optimieren, analysieren Sie die Seitenansichtsdiagramme.

Adobe empfiehlt eine regelmäßige Überprüfung des RUM-Dashboards, insbesondere nach einer Haupt- oder Nebenversion.

Siehe auch [Wer kann von einem echten Benutzerüberwachungsdienst profitieren](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#who-can-benefit-from-rum-service) für weitere Details.
