---
title: AEM Headless-Web-Komponentenbereitstellungen
description: Erfahren Sie mehr über Bereitstellungsüberlegungen für Webkomponenten/reine JS-basierte AEM Headless-Bereitstellungen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---


# AEM Headless-Web-Komponentenbereitstellungen

AEM Headless [Webkomponente](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS-Implementierungen sind reine JavaScript-Apps, die in einem Webbrowser ausgeführt werden und Inhalte auf Headless-AEM-Weise konsumieren und damit interagieren. Web-Komponenten-/JS-Bereitstellungen unterscheiden sich von [SPA](./spa.md) da sie kein robustes SPA Framework verwenden und voraussichtlich im Kontext einer Website eingebettet sein werden, bereitstellen, um Inhalte von AEM zu überdecken.


## Bereitstellungskonfigurationen

Die folgende Bereitstellungskonfiguration muss für Web Component-/JS-Bereitstellungen vorhanden sein.

| Webkomponente/JS-App verbindet sich mit | AEM Author | AEM Publish | AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher-Filter](./configurations/dispatcher-filters.md) | ✘ | ms | ms |
| [Cross-Origin Resource Sharing (CORS)](./configurations/cors.md) | ms | ms | ms |
| [AEM Hosts](./configurations/aem-hosts.md) | ms | ms | ms |

## Beispiel einer Webkomponente

Adobe bietet eine Beispiel-Webkomponente.

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Webkomponente" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Webkomponente">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Webkomponente">Webkomponente</a></p>
                   <p class="is-size-6">Eine Beispiel-Webkomponente, die in reinem JavaScript geschrieben wurde und Inhalte von AEM Headless GraphQL-APIs verbraucht.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
