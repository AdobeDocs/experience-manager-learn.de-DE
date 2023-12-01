---
title: AEM Headless-Bereitstellungen
description: Erfahren Sie mehr über wichtige Aspekte zu AEM Headless-App-Bereitstellungen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 100%

---

# AEM Headless-Bereitstellungen

AEM Headless-Client-Bereitstellungen können in vielen Formen auftreten: als über AEM gehostete SPA, als externe SPA, als Website, als Mobile App oder sogar als Server-zu-Server-Prozess.

Abhängig vom Client und dessen Bereitstellung gelten für AEM Headless-Bereitstellungen unterschiedliche Überlegungen.

## AEM-Service-Architektur

Bevor Sie Überlegungen zur Bereitstellung anstellen, müssen Sie sich mit der logischen Architektur von AEM sowie mit der Trennung und den Rollen der Service-Ebenen bei AEM as a Cloud Service vertraut machen. AEM as a Cloud Service umfasst zwei logische Dienste:

+ __AEM-Autor__ ist der Dienst, bei dem Teams Inhaltsfragmente (und andere Assets) erstellen, zusammen bearbeiten und veröffentlichen.
+ __AEM-Veröffentlichung__ ist der Dienst, der veröffentlichte Inhaltsfragmente (und andere Assets) für den allgemeinen Gebrauch repliziert.
+ __AEM-Vorschau__ ist der Dienst, der der AEM-Veröffentlichung im Verhalten sehr ähnlich ist, aber Inhalte für Vorschau- oder Prüfungszwecke veröffentlicht. AEM-Vorschau ist für interne Zielgruppen und nicht für die allgemeine Bereitstellung von Inhalten vorgesehen. Die Verwendung von AEM-Vorschau ist optional und vom gewünschten Workflow abhängig.

![AEM-Service-Architektur](./assets/overview/aem-service-architecture.png)

Typische Headless-Bereitstellungsarchitektur für AEM as a Cloud Service

AEM Headless-Clients, die in einer Produktionsfunktion arbeiten, interagieren normalerweise mit AEM Publish, der die genehmigten, veröffentlichten Inhalte enthält. Bei Clients, die mit AEM Author interagieren, müssen besondere Sicherheitsvorkehrungen beachtet werden, da AEM Author standardmäßig gesichert ist. Deshalb ist für alle Anfragen eine Autorisierung erforderlich. Außerdem können dort noch nicht abgeschlossene Arbeiten oder nicht genehmigte Inhalte vorhanden sein.

## Headless-Client-Bereitstellungen

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="Einzelseitenanwendung (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="Einzelseitenanwendungen (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="Einzelseitenanwendung (SPA)">Einzelseitenanwendung (SPA)</a></p>
                   <p class="is-size-6">Erfahren Sie mehr über wichtige Aspekte bei der Bereitstellung von Single Page Applications (SPA).</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
<!-- Web component/JS -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web component/JS" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./web-component.md" title="Webkomponente/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Webkomponente/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Webkomponente/JS">Webkomponente/JS</a></p>
               <p class="is-size-6">Erfahren Sie mehr über wichtige Aspekte bei der Bereitstellung von Web-Komponenten und Browser-basierten JavaScript-Headless-Nutzern.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Mobile apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./mobile.md" title="Mobile Apps" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="Mobile Apps">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="Mobile Apps">Mobile App</a></p>
               <p class="is-size-6">Erfahren Sie mehr über wichtige Aspekte bei der Bereitstellung von Mobile Apps.</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                  </a>
           </div>
       </div>
   </div>
</div>
<!-- Server-to-server apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./server-to-server.md" title="Server-zu-Server-Apps" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="Server-zu-Server-Apps">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="Server-zu-Server-Apps">Server-zu-Server-App</a></p>
               <p class="is-size-6">Erfahren Sie mehr über wichtige Aspekte bei der Bereitstellung von Server-zu-Server-Apps</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
           </div>
       </div>
   </div>
</div>
</div>
