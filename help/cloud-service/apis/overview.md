---
title: Übersicht über AEM-APIs
description: Erfahren Sie mehr über die verschiedenen API-Typen in Adobe Experience Manager (AEM) und erhalten Sie einen Überblick über APIs, die auf OpenAPI-Spezifikationen basieren und allgemein als OpenAPI-basierte AEM-APIs bezeichnet werden.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: d5745a17af6b72b1871925dd7c50cbbb152012fe
workflow-type: tm+mt
source-wordcount: '1024'
ht-degree: 2%

---

# Übersicht über AEM-APIs{#aem-apis-overview}

Erfahren Sie mehr über die verschiedenen API-Typen in Adobe Experience Manager (AEM) as a Cloud Service und erhalten Sie einen Überblick über [OpenAPI Specification (OAS)](https://swagger.io/specification/)basierte AEM-APIs, allgemein bekannt als OpenAPI-basierte AEM-APIs.

AEM as a Cloud Service bietet eine breite Palette von APIs zum Erstellen, Lesen, Aktualisieren und Löschen von Inhalten, Assets und Formularen. Diese APIs ermöglichen es Entwicklerinnen und Entwicklern, benutzerdefinierte Programme zu erstellen, die mit AEM interagieren.

Im Folgenden werden die verschiedenen API-Typen in AEM erläutert und die wichtigsten Konzepte für den Zugriff auf Adobe-APIs erklärt.

## Typen von AEM-APIs{#types-of-aem-apis}

AEM bietet sowohl ältere als auch moderne APIs für die Interaktion mit den Autoren- und Veröffentlichungs-Service-Typen.

- **Legacy-APIs**: Die in früheren AEM-Versionen eingeführten Legacy-APIs werden aus Gründen der Abwärtskompatibilität weiterhin unterstützt.

- **Moderne APIs**: Basierend auf der REST- und OpenAPI-Spezifikation folgen diese APIs aktuellen Best Practices für das API-Design und werden für neue Integrationen empfohlen.


| AEM-API-Typ | Technische Daten | Verfügbarkeit | Anwendungsfall | Beispiel |
| --- | --- | --- | --- | --- |
| Herkömmliche (nicht RESTful-)APIs | Sling-Servlets | AEM 6.x, AEM as a Cloud Service | Alte Integrationen, Abwärtskompatibilität | [Query Builder-API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) und andere |
| RESTful-APIs | HTTP, JSON | AEM 6.x, AEM as a Cloud Service | CRUD-Vorgänge, moderne Anwendungen | [Assets HTTP-](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [Workflow-REST-](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [JSON Exporter für Content Services](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) und andere |
| GraphQL-APIs | GraphQL | AEM 6.x, AEM as a Cloud Service | Headless CMS, SPA, Mobile Apps | [GraphQL-API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| OpenAPI-basierte AEM-APIs | REST, OpenAPI | **Nur AEM as a Cloud Service** | API-First-Entwicklung, moderne Anwendungen | [Assets-Autoren-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [Ordner-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [AEM Sites-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [Forms Acrobat Services](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) und andere |

>[!IMPORTANT]
>
>Die OpenAPI-basierten AEM-APIs sind nur in AEM as a Cloud Service verfügbar und nicht mit AEM 6.X kompatibel.

Weitere Informationen zu AEM-APIs finden Sie unter [Adobe Experience Manager as a Cloud Service-APIs](https://developer.adobe.com/experience-cloud/experience-manager-apis/).

Werfen wir einen genaueren Blick auf die OpenAPI-basierten AEM-APIs und die wichtigen Konzepte für den Zugriff auf Adobe-APIs.

## OpenAPI-basierte AEM-APIs{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>OpenAPI-basierte AEM-APIs sind als Teil eines Early-Access-Programms verfügbar. Wenn Sie daran interessiert sind, darauf zuzugreifen, empfehlen wir Ihnen, eine E-Mail an [aem-apis@adobe.com](mailto:aem-apis@adobe.com) mit einer Beschreibung Ihres Anwendungsfalls zu senden.

Die [OpenAPI Spezifikation](https://swagger.io/specification/) (früher bekannt als Swagger) ist ein weit verbreiteter Standard zur Definition von RESTful-APIs. AEM as a Cloud Service bietet mehrere APIs, die auf OpenAPI-Spezifikationen basieren (oder einfach OpenAPI-basierte AEM-APIs). Dies erleichtert die Erstellung benutzerdefinierter Anwendungen, die mit den Autoren- oder Veröffentlichungs-Service-Typen von AEM interagieren. Im Folgenden finden Sie einige Beispiele:

**Sites**

- [Sites-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/): APIs zum Arbeiten mit Inhaltsfragmenten.

**Assets**

- [Ordner-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): APIs zum Arbeiten mit Ordnern wie „Erstellen“, „Auflisten“ und „Löschen“.

- [Assets Author-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): APIs zum Arbeiten mit Assets und den zugehörigen Metadaten.

**Forms**

- [Forms Communications APIs](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): APIs zum Arbeiten mit Formularen und Dokumenten.

In zukünftigen Versionen werden weitere OpenAPI-basierte AEM-APIs hinzugefügt, um weitere Anwendungsfälle zu unterstützen.

## Authentifizierungs-Unterstützung{#authentication-support}

Die OpenAPI-basierten AEM-APIs unterstützen die folgenden Authentifizierungsmethoden:

- **OAuth Server-zu-Server-Anmeldedaten**: Ideal für Backend-Services, die API-Zugriff ohne Benutzerinteraktion benötigen. Sie verwendet den _client_credentials_ Grant-Typ und ermöglicht so eine sichere Zugriffsverwaltung auf Server-Ebene. Weitere Informationen finden Sie unter [OAuth-Server-zu-Server-Anmeldedaten](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Anmeldedaten für OAuth Web App**: Geeignet für Web-Anwendungen mit Frontend- und _Backend_-Komponenten, die im Namen von Benutzern auf AEM-APIs zugreifen. Es verwendet den _authorization_code_-Gewährungstyp, bei dem der Backend-Server Geheimnisse und Token sicher verwaltet. Weitere Informationen finden Sie unter [Anmeldedaten für OAuth-Web-Apps](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **Anmeldedaten für OAuth Single Page App**: Für die SPA-Ausführung im Browser, der ohne Backend-Server im Auftrag von Benutzenden auf APIs zugreifen muss. Sie verwendet den _authorization_code_-Gewährungstyp und verlässt sich auf Client-seitige Sicherheitsmechanismen, die PKCE (Proof Key for Code Exchange) verwenden, um den Autorisierungs-Code-Fluss zu sichern. Weitere Informationen finden Sie unter [OAuth Single Page App Credential](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

### Unterschied zwischen OAuth Server-zu-Server- und OAuth Web App/Single Page App-Anmeldeinformationen{#difference-between-oauth-server-to-server-and-oauth-web-app-single-page-app-credentials}

| | OAuth-Server-zu-Server | OAuth-Benutzerauthentifizierung (Web-App) |
| --- | --- | --- |
| Authentifizierungszweck | Entwickelt für Maschine-zu-Maschine-Interaktionen. | Entwickelt für benutzergesteuerte Interaktionen. |
| Token-Verhalten | Gibt Zugriffstoken aus, die für die Client-Anwendung selbst stehen. | Gibt Zugriffstoken für einen authentifizierten Benutzer aus. |
| Anwendungsfälle | Backend-Services, die API-Zugriff ohne Benutzerinteraktion benötigen. | Web-Anwendungen mit Frontend- und Backend-Komponenten, die im Auftrag von Benutzern auf APIs zugreifen. |
| Sicherheitsaspekte | Sicheres Speichern sensibler Anmeldeinformationen (`client_id`, `client_secret`) in Backend-Systemen. | Die Benutzer authentifizieren sich und erhalten ihr eigenes temporäres Zugriffstoken. Sicheres Speichern sensibler Anmeldeinformationen (`client_id`, `client_secret`) in Backend-Systemen. |
| Genehmigungstyp | _client_credentials_ | _authorization_code_ |

## Zugriff auf Adobe-APIs und zugehörige Konzepte{#accessing-adobe-apis-and-related-concepts}

Bevor Sie auf Adobe-APIs zugreifen, müssen Sie diese Schlüsselkonzepte verstehen:

- **[Adobe Developer Console](https://developer.adobe.com/)**: Der Entwicklungs-Hub für den Zugriff auf Adobe-APIs, SDKs, Echtzeit-Ereignisse, Server-lose Funktionen und mehr. Sie unterscheidet sich vom Developer Console _AEM_, der zum Debugging von AEM-Anwendungen verwendet wird.

- **[Adobe Developer Console-](https://developer.adobe.com/developer-console/docs/guides/projects/)**: Zentraler Ort für die Verwaltung von API-Integrationen, Ereignissen und Laufzeitfunktionen. Hier konfigurieren Sie APIs, legen die Authentifizierung fest und generieren die erforderlichen Anmeldeinformationen.

- **[Produktprofile](https://helpx.adobe.com/de/enterprise/using/manage-product-profiles.html)**: Produktprofile bieten eine Berechtigungsvorgabe, mit der Sie den Benutzer- oder Programmzugriff auf Adobe-Produkte wie AEM, Adobe Target, Adobe Analytics und andere steuern können. Jedem Adobe-Produkt sind vordefinierte Produktprofile zugeordnet.

- **Services**: Services definieren die tatsächlichen Berechtigungen und sind mit dem Produktprofil verknüpft. Um die Berechtigungsvorgabe zu reduzieren oder zu erhöhen, können Sie die Auswahl der mit dem Produktprofil verknüpften Services aufheben oder erhöhen. Auf diese Weise können Sie die Zugriffsebene auf das Produkt und seine APIs steuern. In AEM as a Cloud Service stellen Services Benutzergruppen mit vordefinierten Zugriffssteuerungslisten (ACLs) für Repository-Knoten dar, was eine granulare Berechtigungsverwaltung ermöglicht.

## Nächste Schritte{#next-steps}

Mit einem Verständnis der verschiedenen AEM-API-Typen, einschließlich
OpenAPI-basierte AEM-APIs und die wichtigsten Konzepte für den Zugriff auf Adobe-APIs. Jetzt können Sie mit dem Erstellen benutzerdefinierter Anwendungen beginnen, die mit AEM interagieren.

Beginnen wir mit:

- [Tutorial zum Aufrufen von OpenAPI-basierten AEM-APIs für die Server-zu](invoke-openapi-based-aem-apis.md)Server-Authentifizierung, in dem der Zugriff auf OpenAPI-basierte AEM-APIs (_OAuth-Server-zu-Server-Anmeldeinformationen)_ wird.
- [Tutorial zum Aufrufen von OpenAPI-basierten AEM-APIs mit Benutzerauthentifizierung über eine Web-Anwendung](invoke-openapi-based-aem-apis-from-web-app.md) in dem gezeigt wird, wie von einer Web-Anwendung aus auf OpenAPI _basierte AEM-APIs über Anmeldeinformationen für die OAuth-Web-App zugegriffen werden_.
