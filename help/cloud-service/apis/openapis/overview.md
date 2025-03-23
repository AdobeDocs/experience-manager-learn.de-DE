---
title: OpenAPI-basierte AEM-APIs
description: Erfahren Sie mehr über die OpenAPI-basierten AEM-APIs, einschließlich Authentifizierungsunterstützung, Schlüsselkonzepten und dem Zugriff auf Adobe-APIs.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 0eb0054d-0c0a-4ac0-b7b2-fdaceaa6479b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 2%

---

# OpenAPI-basierte AEM-APIs

>[!IMPORTANT]
>
>Die OpenAPI-basierten AEM-APIs sind nur in AEM as a Cloud Service verfügbar und nicht mit AEM 6.X kompatibel.

Erfahren Sie mehr über die OpenAPI-basierten AEM-APIs, einschließlich Authentifizierungsunterstützung, Schlüsselkonzepten und dem Zugriff auf Adobe-APIs.

Die [OpenAPI Spezifikation](https://swagger.io/specification/) (früher bekannt als Swagger) ist ein weit verbreiteter Standard zur Definition von RESTful-APIs. AEM as a Cloud Service bietet mehrere APIs, die auf OpenAPI-Spezifikationen basieren (oder einfach OpenAPI-basierte AEM-APIs). Dies erleichtert die Erstellung benutzerdefinierter Anwendungen, die mit den Autoren- oder Veröffentlichungs-Service-Typen von AEM interagieren. Im Folgenden finden Sie einige Beispiele:

**Sites**

- [Sites-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/?lang=de): APIs zum Arbeiten mit Inhaltsfragmenten.

**Assets**

- [Ordner-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): APIs zum Arbeiten mit Ordnern wie „Erstellen“, „Auflisten“ und „Löschen“.

- [Assets Author-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): APIs zum Arbeiten mit Assets und den zugehörigen Metadaten.

**Forms**

- [Forms Communications APIs](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): APIs zum Arbeiten mit Formularen und Dokumenten.

In zukünftigen Versionen werden weitere OpenAPI-basierte AEM-APIs hinzugefügt, um weitere Anwendungsfälle zu unterstützen.

>[!AVAILABILITY]
>
>OpenAPI-basierte AEM-APIs sind als Teil eines Early-Access-Programms verfügbar. Wenn Sie daran interessiert sind, darauf zuzugreifen, empfehlen wir Ihnen, eine E-Mail an [aem-apis@adobe.com](mailto:aem-apis@adobe.com) mit einer Beschreibung Ihres Anwendungsfalls zu senden.

## Authentifizierungs-Unterstützung{#authentication-support}

Die OpenAPI-basierten AEM-APIs unterstützen die OAuth 2.0-Authentifizierung, einschließlich der folgenden Gewährungstypen:

- **OAuth Server-zu-Server-Anmeldedaten**: Ideal für Backend-Services, die API-Zugriff ohne Benutzerinteraktion benötigen. Sie verwendet den _client_credentials_ Grant-Typ und ermöglicht so eine sichere Zugriffsverwaltung auf Server-Ebene. Weitere Informationen finden Sie unter [OAuth-Server-zu-Server-Anmeldedaten](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Anmeldedaten für OAuth Web App**: Geeignet für Web-Anwendungen mit Frontend- und _Backend_-Komponenten, die im Namen von Benutzern auf AEM-APIs zugreifen. Es verwendet den _authorization_code_-Gewährungstyp, bei dem der Backend-Server Geheimnisse und Token sicher verwaltet. Weitere Informationen finden Sie unter [Anmeldedaten für OAuth-Web-Apps](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **Anmeldedaten für OAuth Single Page App**: Entwickelt für SPAs, die im Browser ausgeführt werden. Dieser muss auf APIs für einen Benutzer ohne Backend-Server zugreifen. Sie verwendet den _authorization_code_-Gewährungstyp und verlässt sich auf Client-seitige Sicherheitsmechanismen, die PKCE (Proof Key for Code Exchange) verwenden, um den Autorisierungs-Code-Fluss zu sichern. Weitere Informationen finden Sie unter [OAuth Single Page App Credential](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

## Unterschied zwischen OAuth Server-zu-Server- und OAuth Web App/Single Page App-Anmeldeinformationen{#difference-between-oauth-server-to-server-and-oauth-web-app-single-page-app-credentials}

| | OAuth-Server-zu-Server | OAuth-Benutzerauthentifizierung (Web-App) |
| --- | --- | --- |
| Authentifizierungszweck | Entwickelt für Maschine-zu-Maschine-Interaktionen. | Entwickelt für benutzergesteuerte Interaktionen. |
| Token-Verhalten | Gibt Zugriffstoken aus, die für die Client-Anwendung selbst stehen. | Gibt Zugriffstoken für einen authentifizierten Benutzer aus. |
| Anwendungsfälle | Backend-Services, die API-Zugriff ohne Benutzerinteraktion benötigen. | Web-Anwendungen mit Frontend- und Backend-Komponenten, die im Auftrag von Benutzern auf APIs zugreifen. |
| Sicherheitsaspekte | Sicheres Speichern sensibler Anmeldeinformationen (`client_id`, `client_secret`) in Backend-Systemen. | Die Benutzer authentifizieren sich und erhalten ihr eigenes temporäres Zugriffstoken. Sicheres Speichern sensibler Anmeldeinformationen (`client_id`, `client_secret`) in Backend-Systemen. |
| Genehmigungstyp | _client_credentials_ | _authorization_code_ |

## Zugriff auf Adobe-APIs und zugehörige Konzepte{#accessing-adobe-apis-and-related-concepts}

Bevor Sie auf Adobe-APIs zugreifen, müssen Sie diese Schlüsselkonstrukte verstehen:

- **[Adobe Developer Console](https://developer.adobe.com/)**: Der Entwicklungs-Hub für den Zugriff auf Adobe-APIs, SDKs, Echtzeit-Ereignisse, Server-lose Funktionen und mehr. Beachten Sie, dass sie sich von der _AEM_ Developer Console unterscheidet, die zum Debugging von AEM-Programmen verwendet wird.

- **[Adobe Developer Console-](https://developer.adobe.com/developer-console/docs/guides/projects/)**: Zentraler Ort für die Verwaltung von API-Integrationen, Ereignissen und Laufzeitfunktionen. Hier konfigurieren Sie APIs, legen die Authentifizierung fest und generieren die erforderlichen Anmeldeinformationen.

- **[Produktprofile](https://helpx.adobe.com/de/enterprise/using/manage-product-profiles.html)**: Produktprofile bieten eine Berechtigungsvorgabe, mit der Sie den Benutzer- oder Programmzugriff auf Adobe-Produkte wie AEM, Adobe Target, Adobe Analytics und andere steuern können. Jedem Adobe-Produkt sind vordefinierte Produktprofile zugeordnet.

- **Services**: Services definieren die tatsächlichen Berechtigungen und sind mit dem Produktprofil verknüpft. Um die Berechtigungsvorgabe zu reduzieren oder zu erhöhen, können Sie die Auswahl der mit dem Produktprofil verknüpften Services aufheben oder erhöhen. Auf diese Weise können Sie die Zugriffsebene auf das Produkt und seine APIs steuern. In AEM as a Cloud Service stellen Services Benutzergruppen mit vordefinierten Zugriffssteuerungslisten (ACLs) für Repository-Knoten dar, was eine granulare Berechtigungsverwaltung ermöglicht.

## Erste Schritte

Erfahren Sie, wie Sie Ihre AEM as a Cloud Service-Umgebung und ein Adobe Developer Console-Projekt einrichten, um den Zugriff auf die OpenAPI-basierten AEM-APIs zu ermöglichen. Greifen Sie auch über den Browser auf die AEM-API zu, um die Einrichtung zu überprüfen und die Anfrage und die Antwort zu überprüfen.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = Set up OpenAPI-based AEM APIs}
  {description = Learn how to set up your AEM as a Cloud Service environment to enable access to the OpenAPI-based AEM APIs.}
  {image = ./assets/setup/OpenAPI-Setup.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up OpenAPI-based AEM APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="Einrichten von OpenAPI-basierten AEM-APIs" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/OpenAPI-Setup.png" alt="Einrichten von OpenAPI-basierten AEM-APIs"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Einrichten von OpenAPI-basierten AEM-APIs">Einrichten von OpenAPI-basierten AEM-APIs</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie Ihre AEM as a Cloud Service-Umgebung einrichten, um den Zugriff auf die OpenAPI-basierten AEM-APIs zu ermöglichen.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## API-Tutorials

Erfahren Sie, wie Sie die OpenAPI-basierten AEM-APIs mit verschiedenen OAuth-Authentifizierungsmethoden verwenden:

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="Aufrufen der API mithilfe der Server-zu-Server-Authentifizierung" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="Aufrufen der API mithilfe der Server-zu-Server-Authentifizierung"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="Aufrufen der API mithilfe der Server-zu-Server-Authentifizierung">Aufrufen der API mithilfe der Server-zu-Server-Authentifizierung</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs aus einem benutzerdefinierten NodeJS-Programm mithilfe der OAuth-Server-zu-Server-Authentifizierung aufrufen.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="Aufrufen der API mithilfe der Web-App-Authentifizierung" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="Aufrufen der API mithilfe der Web-App-Authentifizierung"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Aufrufen der API mithilfe der Web-App-Authentifizierung">Aufrufen der API mithilfe der Web-App-Authentifizierung</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs über eine benutzerdefinierte Web-Anwendung mithilfe der OAuth-Web-App-Authentifizierung aufrufen.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
