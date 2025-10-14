---
title: Videos und Tutorials zu AEM Sites
description: 'Durchstöbern Sie Videos und Tutorials zu den Funktionen und Möglichkeiten von Adobe Experience Manager Sites. AEM Sites ist eine führende Erlebnis-Management-Plattform. '
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 999bbe542e5c71ae537f93a4c89acf6d304a4292
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 89%

---

# Videos und Tutorials zu AEM Sites {#overview}

{{edge-delivery-services}}

Adobe Experience Manager (AEM) Sites ist die Erlebnis-Management-Plattform von Adobe, die die Erstellung, Verwaltung und Bereitstellung digitaler Erlebnisse ermöglicht, sei es über eine Website, eine App oder einen anderen digitalen Kanal.

## Drei Möglichkeiten zur Bereitstellung von Erlebnissen mit AEM Sites

AEM Sites bietet drei Möglichkeiten zum Aufbauen, Erstellen und Bereitstellen von Erlebnissen. Unabhängig davon, ob Sie Websites erstellen, die Edge-Leistung optimieren oder Headless-Apps nutzen, bietet AEM Sites flexible Optionen, um Ihren Projektanforderungen zu entsprechen:

1. **Edge Delivery Services**-Erlebnisse verwenden Adobes Edge Network, um Inhalte mit hoher Geschwindigkeit und geringer Latenz bereitzustellen. Der Service optimiert automatisch Inhalte für das verbrauchende Gerät, Suchmaschinen und GenAI-Agenten. Autorinnen und Autoren erstellen Inhalte mit dem universellen Editor von Adobe oder der dokumentbasierten Inhaltserstellung.
1. **Headless-/API-First**-Erlebnisse verwenden AEM Publish, um Inhalte als JSON über HTTP-APIs für Mobile Apps, Single Page Applications (SPAs) oder andere Headless-Clients bereitzustellen. Autorinnen und Autoren erstellen Inhalte mit dem Inhaltsfragment-Editor oder dem universellen Editor.
1. **Herkömmliche AEM**-Erlebnisse verwenden AEM Publish, um Inhalte als HTML-Web-Seiten bereitzustellen. Autorinnen und Autoren erstellen Inhalte mit dem Seiteneditor der AEM-Autoreninstanz. Diese Option eignet sich am besten für vorhandene Projekte oder bereits migrierte Projekte.

Alle drei Optionen sind überzeugende Ansätze. Die beste Wahl hängt von Ihrem Anwendungsfall und den betrieblichen Anforderungen ab. Jeder Ansatz ermöglicht es Teams, personalisierte, ansprechende Erlebnisse schnell und skalierbar über jeden Kanal oder jedes Gerät bereitzustellen.

>[!IMPORTANT]
>
> **Edge Delivery Services** ist die neueste und fortschrittlichste Methode, Websites mit AEM bereitzustellen. Es kombiniert die Geschwindigkeit und Skalierbarkeit von Adobe Edge Network mit modernen Authoring-Optionen. Edge Delivery Services wird zwar für neue Projekte empfohlen, AEM Sites unterstützt jedoch weiterhin Headless- und herkömmliche Ansätze, sodass Sie den Pfad wählen können, der Ihren Anforderungen am besten entspricht.

Die folgende Abbildung zeigt die verschiedenen Optionen zum Erstellen von Erlebnissen mit AEM Sites:

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### Vergleichen der Methoden zum Erstellen mit AEM Sites

Die folgende Tabelle bietet einen umfassenden Vergleich der drei Pfade. Der Schwerpunkt liegt dabei auf den Nuancen der Inhaltserstellung und der Bereitstellung für jeden Pfad.

|            | Edge Delivery Services | Headless/API-First | Traditionelles AEM |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **Am besten geeignet für** | Websites mit hohen Anforderungen an Traffic, Leistung und Skalierbarkeit | Mobile Apps, SPAs und andere Headless-Anwendungen | Bestehende Projekte oder migrierte Projekte |
| **Authoring-Tools** | Dokumentenbasiertes Authoring, universeller Editor, Seiteneditor | Inhaltsfragmente, universeller Editor | Seiteneditor, universeller Editor |
| **Speicher für erstellte Inhalte** | Dokumente oder AEM Author (JCR) | AEM Author (JCR) | AEM Author (JCR) |
| **Versand** | Edge Delivery Services | AEM Publish (mit Adobe CDN + Dispatcher) | AEM Publish (mit Adobe CDN + Dispatcher) |
| **Speicher für Versandinhalte** | Edge Delivery Services | AEM Publish (JCR) | AEM Publish (JCR) |
| **Bereitstellungsformat** | HTML  | JSON | HTML  |
| **Entwicklungstechnologie** | JavaScript, CSS | Beliebig (z. B. Swift, React usw.) | Java™, HTL, JavaScript, CSS |
| **Unterstützung von Such-Bots und GenAI-Agenten** | Optimiert für Bots, Suchmaschinen und GenAI-Agenten | Funktioniert für Bots und Agenten, erfordert jedoch möglicherweise SSR oder ein zusätzliches Setup | Geeignet für Bots, aber die Leistung kann im Vergleich zu Edge Delivery Services langsamer sein |

## Migration von AMS oder On-Premise

Wenn Sie von AMS oder On-Premise (OTP) zu AEM as a Cloud Service migrieren, empfiehlt Ihnen Adobe, den direkten Wechsel zu Edge Delivery Services in Erwägung zu ziehen. Dieser Aufwand ist in der Regel nicht größer als bei der Migration zur AEM as a Cloud Service Publish-Veröffentlichung und bietet gleichzeitig eine schnellere Leistung und höhere Skalierbarkeit. Wenn Sie sich entscheiden, dass Edge Delivery Services derzeit nicht die richtige Wahl für Sie ist, oder wenn die anderen Ansätze Ihren Anforderungen besser entsprechen, bleiben sie vollständig unterstützt und gültige Optionen für Ihr Projekt.

## Tutorials

Lernen Sie die drei Ansätze zum Erstellen mit AEM Sites genauer kennen. Die folgenden Tutorials zeigen Ihnen Schritt für Schritt, wie die einzelnen Optionen funktionieren, welche Tools Sie benötigen und wann Sie sie verwenden müssen.

<!-- CARDS

* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with Edge Delivery Services.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
* https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional AEM - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Handbücher zu Edge Delivery Services" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Handbücher zu Edge Delivery Services"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Handbücher zu Edge Delivery Services">Edge Delivery Services – Handbücher</a>
                    </p>
                    <p class="is-size-6">Erkunden Sie Edge Delivery Services mit umfassenden Handbüchern. Die Build-, Veröffentlichungs- und Launch-Handbücher decken alles ab, was Sie für die ersten Schritte mit Edge Delivery Services benötigen.</p>
                </div>
                <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Headless/API-First - Tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="Headless/API-First – Tutorials" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="Headless/API-First – Tutorials"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="Headless/API-First – Tutorials">Headless/API-First – Tutorials</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie Headless-Anwendungen erstellen, die auf AEM-Inhalten basieren. Die Tutorials behandeln Frameworks wie iOS, Android und React – wählen Sie das aus, was zu Ihrem Stapel passt.</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional AEM - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="Traditionelles AEM – WKND-Tutorial" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="Traditionelles AEM – WKND-Tutorial"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="Traditionelles AEM – WKND-Tutorial">Traditionelles AEM – WKND-Tutorial</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie im WKND-Tutorial, wie Sie ein Beispielprojekt für AEM Sites erstellen. Dieses Handbuch führt Sie durch die Einrichtung von Projekten, Kernkomponenten, bearbeitbare Vorlagen, Client-seitige Bibliotheken und die Entwicklung von Komponenten.</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Zusätzliche Ressourcen

* [AEM Sites-Dokumentation zum Authoring](https://experienceleague.adobe.com/de/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [AEM Sites-Dokumentation zur Entwicklung](https://experienceleague.adobe.com/de/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [AEM Sites-Dokumentation zur Verwaltung](https://experienceleague.adobe.com/de/docs/experience-manager-65/content/sites/administering/home)
* [AEM Sites-Dokumentation zur Bereitstellung](https://experienceleague.adobe.com/de/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [Tutorials zu AEM as a Cloud Service](/help/cloud-service/overview.md)
* [Tutorials zu AEM Assets](/help/assets/overview.md)
* [Tutorials zu AEM Forms](/help/forms/overview.md)
* [Grundlegende Tutorials zu AEM](/help/foundation/overview.md)
