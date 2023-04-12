---
title: Bereitstellungen von AEM Headless-Web-Komponenten
description: Erfahren Sie mehr über Bereitstellungsüberlegungen für Web-Komponenten-/rein JS-basierte AEM Headless-Bereitstellungen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: ht
source-wordcount: '161'
ht-degree: 100%

---


# Bereitstellungen von AEM Headless-Web-Komponenten

AEM Headless-[Web-Komponenten](https://developer.mozilla.org/de-DE/docs/Web/Web_Components)-/JS-Bereitstellungen sind reine JavaScript-Apps, die in einem Webbrowser ausgeführt werden und Inhalte in AEM auf Headless-Weise nutzen und damit interagieren. Web-Komponenten-/JS-Bereitstellungen unterscheiden sich von [SPA-Bereitstellungen](./spa.md) dahingehend, dass sie kein robustes SPA-Framework verwenden und normalerweise im Kontext einer Website eingebettet werden, um Inhalte von AEM zur Verfügung zu stellen.


## Implementierungskonfigurationen

Die folgende Bereitstellungskonfiguration muss für Web-Komponenten-/JS-Bereitstellungen vorhanden sein.

| Web-Komponenten-/JS-App-Verbindung zu | AEM Author | AEM Publish | AEM-Vorschau |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher-Filter](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Cross-Origin Resource Sharing (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM-Hosts](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Beispielhafte Web-Komponente

Adobe stellt eine beispielhafte Web-Komponente zur Verfügung.

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Web-Komponente" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Web-Komponente">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Web-Komponente">Web-Komponente</a></p>
                   <p class="is-size-6">Eine beispielhafte Web-Komponente, die in reinem JavaScript geschrieben wurde und Inhalte von AEM Headless-GraphQL-APIs nutzt.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
 </a> 
               </div>
           </div>
       </div>
    </div>
</div>
