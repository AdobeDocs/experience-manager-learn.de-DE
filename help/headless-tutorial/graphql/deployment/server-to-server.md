---
title: Bereitstellungen von AEM Headless von Server zu Server
description: Erfahren Sie mehr über Bereitstellungsüberlegungen für AEM Headless-Bereitstellungen von Server zu Server.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 100%

---

# Bereitstellungen von AEM Headless von Server zu Server

AEM Headless-Server-zu-Server-Bereitstellungen umfassen Server-seitige Anwendungen oder Prozesse, die Inhalte in AEM Headless nutzen und mit ihnen interagieren.

Server-zu-Server-Bereitstellungen erfordern eine minimale Konfiguration, da HTTP-Verbindungen zu AEM Headless-APIs nicht im Kontext eines Browsers initiiert werden.

## Bereitstellungskonfigurationen

Die folgende Bereitstellungskonfiguration muss für Server-zu-Server-App-Bereitstellungen vorhanden sein.

| Die Server-zu-Server-App stellt eine Verbindung zu | AEM Author | AEM Publish | AEM-Vorschau |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher-Filter](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Cross-Origin Resource Sharing (CORS) | ✘ | ✘ | ✘ |
| [AEM-Hosts](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Zulassungsanforderungen

Autorisierte Anfragen an AEM GraphQL-APIs treten typischerweise im Kontext von Server-zu-Server-Anwendungen auf, da andere Anwendungstypen wie [Einzelseiten-Apps](./spa.md), [Mobile](./mobile.md) oder [Web-Komponenten](./web-component.md) typischerweise eine Autorisierung verwenden, da es schwierig ist, die Anmeldeinformationen zu sichern.

Verwenden Sie beim Zulassen von Anforderungen an AEM as a Cloud Service [Authentifizierung von Service-Anmeldeinformationen-basierten Token](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=de). Weitere Informationen zum Authentifizieren von Anforderungen an AEM as a Cloud Service finden Sie in dem [Token-basierten Authentifizierungs-Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=de). In diesem Tutorial wird die Token-basierte Authentifizierung mithilfe von [AEM Assets HTTP-APIs](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html?lang=de) untersucht. Für Apps, die mit AEM Headless-GraphQL-APIs interagieren, gelten jedoch dieselben Konzepte und Ansätze.

## Beispiel einer Server-zu-Server-App

Adobe stellt ein Beispiel für eine in Node.js codierte Server-zu-Server-App bereit.

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="Server-zu-Server-App" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="Server-zu-Server-App">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="Server-zu-Server-App">Server-zu-Server-App</a></p>
                   <p class="is-size-6">Ein Beispiel für eine in Node.js geschriebene Server-zu-Server-App, die Inhalte von AEM Headless-GraphQL-APIs konsumiert.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
 </a>
               </div>
           </div>
       </div>
    </div>
</div>
