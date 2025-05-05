---
title: Übersicht über AEM-APIs
description: Erfahren Sie mehr über die verschiedenen API-Typen in Adobe Experience Manager (AEM) und lernen Sie kennen, welche APIs Sie für Ihre Integration auswählen können.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17425
thumbnail: KT-17425.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: 7cd9efb62d1afdcc089e1e6260d6cf2fc5495afe
workflow-type: tm+mt
source-wordcount: '999'
ht-degree: 5%

---

# Übersicht über AEM-APIs{#aem-apis-overview}

Erfahren Sie mehr über die verschiedenen API-Typen in Adobe Experience Manager (AEM) und lernen Sie kennen, welche APIs Sie für Ihre Integration auswählen können.

Zum Erstellen, Lesen, Aktualisieren und Löschen von Inhalten, Assets und Formularen in AEM können Entwicklerinnen und Entwickler eine Vielzahl von APIs verwenden. Diese APIs ermöglichen es Entwickelnden, benutzerdefinierte Programme zu erstellen, die mit AEM interagieren.

Im Folgenden werden die verschiedenen API-Typen in AEM untersucht und es wird erklärt, welche API für Ihre Integration ausgewählt werden soll.

## Typen von AEM-APIs{#types-of-aem-apis}

AEM bietet folgende APIs für die Interaktion mit den Autoren- und Veröffentlichungs-Service-Typen.

| AEM-API-Typ | Beschreibung | Verfügbarkeit | Anwendungsfall | API-Beispiele |
| --- | --- | --- | --- | --- |
| OpenAPI-basierte AEM-APIs | Standardisierte, maschinenlesbare APIs für Assets, Sites und Forms. | **Nur AEM as a Cloud Service** | API-First-Entwicklung, moderne Anwendungen | [Assets Author-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [Folders-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [AEM Sites-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/?lang=de), [Forms Document Services-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) und andere |
| RESTful-APIs | Herkömmliche REST-Endpunkte für die Interaktion mit AEM-Ressourcen. | AEM 6.x, AEM as a Cloud Service | CRUD-Vorgänge, moderne Anwendungen | [Assets HTTP-](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [Workflow-REST-](https://experienceleague.adobe.com/de/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [JSON Exporter für Content Services](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) und andere |
| GraphQL-APIs | Optimiert für das effiziente Abrufen strukturierter Inhalte mit flexiblen Abfragen. | AEM 6.x, AEM as a Cloud Service | Headless-CMS, SPAs, Mobile Apps | [GraphQL-API](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| Herkömmliche (nicht RESTful-)APIs | Ältere APIs wie JCR, Sling-Modelle, Query Builder und andere. | AEM 6.x, AEM as a Cloud Service | Alte Integrationen, Abwärtskompatibilität | [Query Builder-API](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) und andere |

Weitere Informationen finden Sie auf der Seite [Adobe Experience Manager as a Cloud Service-APIs](https://developer.adobe.com/experience-cloud/experience-manager-apis/) .

## Welche API ausgewählt werden soll{#which-api-to-choose}

Beachten Sie bei der Auswahl einer API für Ihre Integration die folgenden Faktoren:

- **Anwendungsfall**: Ermitteln Sie, ob die AEM-API Ihren Anwendungsfall unterstützt. Verwenden Sie _OpenAPI-basierte AEM-APIs_ da sie einen standardisierten, modernen Ansatz für die Interaktion mit AEM bieten. Wenn keine OpenAPI-basierten APIs verfügbar sind, sollten Sie RESTful-APIs oder GraphQL-APIs und als letztes Mittel herkömmliche APIs verwenden.

- **Kompatibilität**: Stellen Sie sicher, dass die ausgewählte API mit Ihrer AEM-Version kompatibel ist. Beispielsweise sind _OpenAPI-basierte AEM-APIs ausschließlich für AEM as a Cloud Service_ und in AEM 6.X nicht verfügbar.

- **AEM-Service-Typ: Autor vs. Veröffentlichung**: Die Auswahl der API hängt auch davon ab, ob sie auf dem Autoren- oder Veröffentlichungs-Service ausgeführt wird, da die Zugriffsmodelle unterschiedlich sind. Der AEM-Autoren-Service wird für die Inhaltserstellung verwendet und erfordert immer eine Authentifizierung. Der AEM-Veröffentlichungs-Service wird für die Bereitstellung von Inhalten verwendet und erfordert je nach Anwendungsfall möglicherweise keine Authentifizierung.

- **Authentifizierung**: Stellen Sie sicher, dass die API die Authentifizierungsmethode unterstützt, die Sie verwenden möchten. Zum Beispiel:
   - **OpenAPI-basierte AEM-APIs**: unterstützen die OAuth 2.0-Authentifizierung, einschließlich der Gewährungstypen Client-Anmeldeinformationen (Server-zu-Server), Autorisierungs-Code (Web-App) und Korrekturabzugsschlüssel für Code Exchange (Single Page App). Andere AEM-APIs unterstützen keine OAuth 2.0-Authentifizierung.
   - **RESTful-APIs**: unterstützen die JSON Web Token (JWT)-Authentifizierung, auch als Token-basierte Authentifizierung bezeichnet.

## Unterschied zwischen JSON Web Token (JWT) und OAuth 2.0{#difference-between-jwt-and-oauth}

Vergleichen wir JSON Web Token (JWT) und OAuth 2.0, zwei gängige Authentifizierungsmechanismen, die in AEM-APIs verwendet werden:

| Funktion | JSON Web Token (JWT) | OAuth 2.0 |
| --- | --- | --- |
| Verwendet in | RESTful-APIs | OpenAPI-basierte AEM-APIs (in RESTful- oder anderen APIs nicht unterstützt) |
| Zweck | Service-Authentifizierung | Benutzer- oder Dienstauthentifizierung |
| Benutzerinteraktion | Keine Benutzerinteraktion erforderlich | Benutzerinteraktion, die für die Gewährungstypen Autorisierungs-Code und Einzelseiten-App erforderlich ist |
| Am besten geeignet für | Server-zu-Server-API-Aufrufe | Sicherer, zulässiger Zugriff für Apps und Benutzer |
| Erforderliche Informationen | Privater Schlüssel für das Signieren von JWT | Client-ID und Client-Geheimnis für OAuth 2.0 |
| Token-Ablauf | Kurzlebig, muss oft aktualisiert werden | Das Zugriffs-Token ist kurzlebig. Das Aktualisierungs-Token ist langlebig und wird zum Abrufen eines neuen Zugriffs-Tokens verwendet |
| Zugangsdaten-Management | [AEM Developer Console](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console) | [Adobe-Entwicklerkonsole](https://developer.adobe.com/developer-console/) |

## OpenAPI-basierte AEM-APIs

Erfahren Sie mehr über die OpenAPI-basierten AEM-APIs und die wichtigen Konzepte für den Zugriff auf Adobe-APIs im [OpenAPI-basierten AEM-APIs](./openapis/overview.md)-Handbuch.

### Anwendungsfälle

<!-- CARDS
{target = _self}

* ./openapis/use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./openapis/assets/s2s/OAuth-S2S.png}
* ./openapis/use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./openapis/assets/web-app/OAuth-WebApp.png} 
* ./openapis/use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using OAuth Single Page App}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth 2.0 PKCE flow.}
  {image = ./openapis/assets/spa/OAuth-SPA.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" title="Aufrufen der API mithilfe der Server-zu-Server-Authentifizierung" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/s2s/OAuth-S2S.png" alt="Aufrufen der API mithilfe der Server-zu-Server-Authentifizierung"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="Aufrufen der API mithilfe der Server-zu-Server-Authentifizierung">Aufrufen der API mithilfe der Server-zu-Server-Authentifizierung</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs aus einem benutzerdefinierten NodeJS-Programm mithilfe der OAuth-Server-zu-Server-Authentifizierung aufrufen.</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" title="Aufrufen der API mithilfe der Web-App-Authentifizierung" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/web-app/OAuth-WebApp.png" alt="Aufrufen der API mithilfe der Web-App-Authentifizierung"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Aufrufen der API mithilfe der Web-App-Authentifizierung">Aufrufen der API mithilfe der Web-App-Authentifizierung</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs über eine benutzerdefinierte Web-Anwendung mithilfe der OAuth-Web-App-Authentifizierung aufrufen.</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using OAuth Single Page App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" title="Aufrufen der API mithilfe der OAuth-Einzelseiten-App" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/spa/OAuth-SPA.png" alt="Aufrufen der API mithilfe der OAuth-Einzelseiten-App"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="Aufrufen der API mithilfe der OAuth-Einzelseiten-App">Aufrufen der API mithilfe der OAuth-Einzelseiten-App</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs aus einer benutzerdefinierten Single Page App (SPA) mithilfe des OAuth 2.0 PKCE-Flusses aufrufen.</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## GraphQL-APIs - Beispiele

Weitere Informationen zu den GraphQL-APIs und deren Verwendung finden Sie unter [Erste Schritte mit AEM Headless - GraphQL](https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview)

### Anwendungsfälle

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app
  {title = Single Page Application (SPA)}
  {description = Learn how to build a Single Page Application (SPA) that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/react-app-card.png}
* https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps
  {title = Mobile App}
  {description = Learn how to build a mobile app that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/ios-app-card.png}
* https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component
  {title = Web Component}
  {description = Learn how to build a web component that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/web-component-card.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single Page Application (SPA)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" title="Single Page Application (SPA)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/react-app-card.png" alt="Single Page Application (SPA)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" title="Single Page Application (SPA)">Single Page Application (SPA)</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie mithilfe von GraphQL-APIs eine Single Page Application (SPA) erstellen, die Inhalte aus AEM abruft.</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" title="Mobile App" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/ios-app-card.png" alt="Mobile App"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" title="Mobile App">Mobile App</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie mithilfe von GraphQL-APIs eine Mobile App erstellen, die Inhalte aus AEM abruft.</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" title="Web-Komponente" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-component-card.png" alt="Web-Komponente"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" title="Web-Komponente">Web-Komponente</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie mithilfe von GraphQL-APIs eine Web-Komponente erstellen, die Inhalte aus AEM abruft.</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## RESTful-APIs - Beispiele

Erfahren Sie mehr über die RESTful-APIs, z. B. [Assets HTTP](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)API und [JSON Exporter](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter).

### Anwendungsfälle

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview
  {title = Using Content Services for Headless App}
  {description = Learn how to build a native mobile app that fetches content from AEM using Content Services RESTful APIs.}
  {image = ./assets/RESTful-Content-Service.png}
* https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview
  {title = Token-based Authentication for RESTful APIs}
  {description = Learn how to invoke RESTful APIs using JSON Web Token (JWT) authentication.}
  {image = ./assets/RESTful-TokenAuth.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Using Content Services for Headless App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" title="Verwenden von Content Services für die Headless-App" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-Content-Service.png" alt="Verwenden von Content Services für die Headless-App"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" title="Verwenden von Content Services für die Headless-App">Verwenden von Content Services für die Headless-App</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie mithilfe von Content Services RESTful-APIs eine native Mobile App erstellen, die Inhalte aus AEM abruft.</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Token-based Authentication for RESTful APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" title="Token-basierte Authentifizierung für RESTful-APIs" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-TokenAuth.png" alt="Token-basierte Authentifizierung für RESTful-APIs"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" title="Token-basierte Authentifizierung für RESTful-APIs">Token-basierte Authentifizierung für RESTful-APIs</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie RESTful-APIs mithilfe der JSON Web Token (JWT)-Authentifizierung aufrufen.</p>
                </div>
                <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


