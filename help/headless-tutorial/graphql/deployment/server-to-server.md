---
title: AEM Headless-Server-zu-Server-Implementierungen
description: Erfahren Sie mehr über Bereitstellungsaspekte bei Server-zu-Server-AEM Headless-Bereitstellungen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10798
thumbnail: kt-10798.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# AEM Headless-Server-zu-Server-Implementierungen

AEM Headless-Server-zu-Server-Bereitstellungen umfassen Server-seitige Anwendungen oder Prozesse, die Inhalte in AEM Headless nutzen und mit ihnen interagieren.

Server-zu-Server-Implementierungen erfordern eine minimale Konfiguration, da HTTP-Verbindungen zu AEM Headless-APIs nicht im Kontext eines Browsers initiiert werden.

## Bereitstellungskonfigurationen

Die folgende Bereitstellungskonfiguration muss für Server-zu-Server-App-Bereitstellungen vorhanden sein.

| Die Server-zu-Server-App stellt eine Verbindung zu | AEM Author | AEM Publish | AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher-Filter](./configurations/dispatcher-filters.md) | ✘ | ms | ms |
| Cross-Origin Resource Sharing (CORS) | ✘ | ✘ | ✘ |
| [AEM Hosts](./configurations/aem-hosts.md) | ms | ms | ms |

## Zulassungsanforderungen

Autorisierte Anforderungen an GraphQL-APIs, die normalerweise im Kontext von Server-zu-Server-Apps auftreten, AEM, da andere App-Typen, z. B. [Einzelseiten-Apps](./spa.md), [mobile](./mobile.md)oder [Webkomponenten](./web-component.md)verwenden Sie in der Regel die Autorisierung, da es schwierig ist, die Anmeldeinformationen zu sichern.

Verwenden Sie beim Zulassen von Anforderungen an AEM as a Cloud Service [Authentifizierung von Service-Anmeldeinformationen-basierten Token](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). Weitere Informationen zum Authentifizieren von Anforderungen an AEM as a Cloud Service finden Sie in der [Token-basiertes Authentifizierungs-Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). In diesem Tutorial wird die Token-basierte Authentifizierung mithilfe von [AEM Assets HTTP-APIs](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) Es gelten jedoch dieselben Konzepte und Ansätze für Apps, die mit AEM Headless GraphQL-APIs interagieren.

## Beispiel einer Server-zu-Server-App

Adobe bietet eine Beispielanwendung vom Server zum Server, die in Node.js codiert ist.

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
                   <p class="is-size-6">Eine Beispielanwendung vom Server zum Server, geschrieben in Node.js, die Inhalte von AEM Headless GraphQL-APIs verbraucht.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>