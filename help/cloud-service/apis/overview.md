---
title: Übersicht über AEM-APIs
description: Erfahren Sie mehr über die verschiedenen API-Typen in Adobe Experience Manager (AEM) und erhalten Sie einen Überblick über OpenAPI Specification-basierte APIs, die häufig als OpenAPI-basierte AEM-APIs bezeichnet werden.
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
source-git-commit: 316e08e6647d6fd731cd49ae1bc139ce57c3a7f4
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 2%

---

# Übersicht über AEM-APIs{#aem-apis-overview}

Lernen Sie die verschiedenen API-Typen in Adobe Experience Manager (AEM) as a Cloud Service kennen und erhalten Sie einen Überblick über die auf [OpenAPI Specification (OAS)](https://swagger.io/specification/) basierenden AEM-APIs, die häufig als OpenAPI-basierte AEM APIs bezeichnet werden.

AEM as a Cloud Service bietet eine Vielzahl von APIs zum Erstellen, Lesen, Aktualisieren und Löschen von Inhalten, Assets und Formularen. Mit diesen APIs können Entwickler benutzerdefinierte Programme erstellen, die mit AEM interagieren.

Im Folgenden werden die verschiedenen API-Typen in AEM erläutert und die Schlüsselkonzepte für den Zugriff auf Adobe-APIs erläutert.

## Typen von AEM-APIs{#types-of-aem-apis}

AEM bietet sowohl alte als auch moderne APIs für die Interaktion mit den Typen der Autoren- und Veröffentlichungsdienste.

- **Legacy-APIs**: In früheren AEM-Versionen eingeführt, werden ältere APIs weiterhin aus Gründen der Abwärtskompatibilität unterstützt.

- **Moderne APIs**: Basierend auf der REST- und OpenAPI-Spezifikation folgen diese APIs den aktuellen Best Practices beim API-Design und werden für neue Integrationen empfohlen.


| AEM API-Typ | Spezifikationen | Verfügbarkeit | Anwendungsfall | Beispiel |
| --- | --- | --- | --- | --- |
| Herkömmliche (Nicht-RESTful) APIs | Sling-Servlets | AEM 6.X, AEM as a Cloud Service | Alte Integrationen, Abwärtskompatibilität | [Query Builder-API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) und andere |
| RESTful-APIs | HTTP, JSON | AEM 6.X, AEM as a Cloud Service | CRUD-Vorgänge, moderne Anwendungen | [Assets HTTP API](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [Workflow REST API](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [JSON Exporter for Content Services](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) und andere |
| GraphQL-APIs | GraphQL | AEM 6.X, AEM as a Cloud Service | Headless CMS, SPA, mobile Apps | [GraphQL-API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| OpenAPI-basierte AEM-APIs | REST, OpenAPI | **Nur AEM as a Cloud Service** | API-Erste Entwicklung, moderne Anwendungen | [Assets-Autoren-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [Ordner-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [AEM Sites-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [Forms Acrobat Services](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) und andere |

>[!IMPORTANT]
>
>Die OpenAPI-basierten AEM-APIs sind nur in AEM as a Cloud Service verfügbar und nicht mit AEM 6.X kompatibel.

Weitere Informationen zu AEM-APIs finden Sie in den [Adobe Experience Manager as a Cloud Service-APIs](https://developer.adobe.com/experience-cloud/experience-manager-apis/).

Im Folgenden werden die OpenAPI-basierten AEM-APIs und die wichtigen Konzepte zum Zugriff auf Adobe-APIs vorgestellt.

## OpenAPI-basierte AEM-APIs{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>OpenAPI-basierte AEM-APIs sind im Rahmen eines Programms für frühzeitigen Zugriff verfügbar. Wenn Sie daran interessiert sind, darauf zuzugreifen, empfehlen wir Ihnen, [aem-apis@adobe.com](mailto:aem-apis@adobe.com) mit einer Beschreibung Ihres Anwendungsfalls per E-Mail zu versenden.

Die [OpenAPI-Spezifikation](https://swagger.io/specification/) (früher Swagger genannt) ist ein häufig verwendeter Standard für die Definition von RESTful-APIs. AEM as a Cloud Service bietet verschiedene auf OpenAPI-Spezifikationen basierende APIs (oder einfach OpenAPI-basierte AEM-APIs), die die Erstellung benutzerdefinierter Programme erleichtern, die mit AEM Autoren- oder Veröffentlichungsdiensttypen interagieren. Im Folgenden finden Sie einige Beispiele:

**Sites**

- [Sites-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/): APIs zum Arbeiten mit Inhaltsfragmenten.

**Assets**

- [Ordner-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): APIs zum Arbeiten mit Ordnern wie Erstellen, Auflisten und Löschen von Ordnern.

- [Assets-Autoren-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): APIs zum Arbeiten mit Assets und den zugehörigen Metadaten.

**Forms**

- [Forms Communications-APIs](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): APIs zum Arbeiten mit Formularen und Dokumenten.

In zukünftigen Versionen werden weitere OpenAPI-basierte AEM-APIs hinzugefügt, um zusätzliche Anwendungsfälle zu unterstützen.

## Authentifizierungsunterstützung{#authentication-support}

Die OpenAPI-basierten AEM-APIs unterstützen die folgenden Authentifizierungsmethoden:

- **OAuth Server-zu-Server-Anmeldedaten**: Ideal für Backend-Dienste, die API-Zugriff ohne Benutzerinteraktion benötigen. Es wird der Grant-Typ _client_credentials_ verwendet, um eine sichere Zugriffsverwaltung auf Serverebene zu ermöglichen. Weitere Informationen finden Sie unter [OAuth Server-zu-Server-Anmeldedaten](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **OAuth Web App Credential**: Geeignet für Webanwendungen mit Frontend- und _Backend_-Komponenten, die im Namen von Benutzern auf AEM APIs zugreifen. Es wird der Grant-Typ _authorization_code_ verwendet, bei dem der Backend-Server Geheimnisse und Token sicher verwaltet. Weitere Informationen finden Sie unter [OAuth Web App Credential](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **OAuth Single Page App Credential**: Wurde für SPA Ausführung im Browser entwickelt, der auf APIs im Namen eines Benutzers ohne Backend-Server zugreifen muss. Es verwendet den Grant-Typ _authorization_code_ und verlässt sich auf clientseitige Sicherheitsmechanismen mithilfe von PKCE (Proof Key for Code Exchange), um den Autorisierungscode-Fluss zu sichern. Weitere Informationen finden Sie unter [OAuth Single Page App Credential](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

## Zugriff auf Adobe-APIs und zugehörige Konzepte{#accessing-adobe-apis-and-related-concepts}

Bevor Sie auf Adobe-APIs zugreifen, müssen Sie sich mit diesen Schlüsselkonzepten vertraut machen:

- **[Adobe Developer Console](https://developer.adobe.com/)**: Der Entwickler-Hub für den Zugriff auf Adobe-APIs, SDKs, Echtzeit-Ereignisse, Server-lose Funktionen und mehr. Beachten Sie, dass es sich von der Developer Console _AEM_ unterscheidet, die zum Debugging von AEM Anwendungen verwendet wird.

- **[Adobe Developer Console-Projekt](https://developer.adobe.com/developer-console/docs/guides/projects/)**: Zentraler Ort für die Verwaltung von API-Integrationen, Ereignissen und Laufzeitfunktionen. Hier konfigurieren Sie APIs, legen die Authentifizierung fest und generieren erforderliche Anmeldeinformationen.

- **[Produktprofile](https://helpx.adobe.com/de/enterprise/using/manage-product-profiles.html)**: Produktprofile bieten eine Berechtigungsvorgabe, mit der Sie den Benutzer- oder Anwendungszugriff auf Adobe-Produkte wie AEM, Adobe Target, Adobe Analytics und andere steuern können. Jedem Adobe-Produkt sind vordefinierte Produktprofile zugeordnet.

- **Dienste**: Dienste definieren die tatsächlichen Berechtigungen und sind mit dem Produktprofil verknüpft. Um die Berechtigungsvorgabe zu reduzieren oder zu erhöhen, können Sie die mit dem Produktprofil verknüpften Dienste deaktivieren oder auswählen. So können Sie den Umfang des Zugriffs auf das Produkt und seine APIs steuern. In AEM as a Cloud Service stellen Dienste Benutzergruppen mit vordefinierten Zugriffssteuerungslisten (ACLs) für Repository-Knoten dar, was eine granulare Berechtigungsverwaltung ermöglicht.

## Nächste Schritte{#next-steps}

Verstehen der verschiedenen AEM API-Typen, einschließlich
OpenAPI-basierte AEM-APIs und die Schlüsselkonzepte für den Zugriff auf Adobe-APIs sind jetzt bereit, mit der Erstellung benutzerdefinierter Programme zu beginnen, die mit AEM interagieren.

Beginnen wir mit dem Tutorial [Aufrufen von OpenAPI-basierten AEM-APIs](invoke-openapi-based-aem-apis.md) .
