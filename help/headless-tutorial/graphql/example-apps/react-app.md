---
title: React-App - AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese React-Anwendung zeigt, wie Sie mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen Inhalte abfragen können.
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-11-09T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 758fa40240b12f5bfa83ac5c0300b71f41e2326d
workflow-type: tm+mt
source-wordcount: '919'
ht-degree: 7%

---

# React App{#react-app}

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese React-Anwendung zeigt, wie Sie mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen Inhalte abfragen können. Der AEM Headless-Client für JavaScript wird verwendet, um die von GraphQL gespeicherten Abfragen auszuführen, die die App unterstützen.

![React-App mit AEM Headless](./assets/react-app/react-app.png)

Anzeigen der [Quellcode auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [Umfassendes Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=de) , die beschreiben, wie diese React-App erstellt wurde, verfügbar ist.

## Voraussetzungen {#prerequisites}

Die folgenden Tools sollten lokal installiert werden:

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM

Die React-Anwendung funktioniert mit den folgenden AEM Bereitstellungsoptionen. Für alle Implementierungen ist die [WKND Site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0) installiert werden.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=de)
+ Lokales Setup mit [das AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de?lang=de#install-local-aem-instances)

Die React-Anwendung ist für die Verbindung mit einer __AEM-Veröffentlichung__ -Umgebung, kann jedoch Inhalte von der AEM-Autoreninstanz stammen, wenn die Authentifizierung in der Konfiguration der React-Anwendung bereitgestellt wird.

## Informationen zur Verwendung

1. Klonen Sie die `adobe/aem-guides-wknd-graphql` repository:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bearbeiten Sie die `aem-guides-wknd-graphql/react-app/.env.development` Datei und Set `REACT_APP_HOST_URI` , um auf Ihre AEM zu verweisen.

   Aktualisieren Sie die Authentifizierungsmethode, wenn Sie eine Verbindung zu einer Autoreninstanz herstellen.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=basic
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=dev-token
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

1. Öffnen Sie ein Terminal und führen Sie die Befehle aus:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Ein neues Browser-Fenster sollte in [http://localhost:3000](http://localhost:3000)
1. Eine Liste der Abenteuer von der WKND-Referenz-Website sollte auf der Anwendung angezeigt werden.

## Der Code

Nachstehend finden Sie eine Zusammenfassung dazu, wie die React-Anwendung erstellt wurde, wie sie eine Verbindung zu AEM Headless herstellt, um mithilfe von durch GraphQL gespeicherten Abfragen Inhalte abzurufen, und wie diese Daten dargestellt werden. Den vollständigen Code finden Sie unter [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Beständige Abfragen

Gemäß AEM Best Practices für Headless verwendet die React-Anwendung AEM von GraphQL gespeicherten Abfragen, um Abenteuerdaten abzufragen. Die Anwendung verwendet zwei persistente Abfragen:

+ `wknd/adventures-all` persistente Abfrage, die alle Abenteuer in AEM mit einer gekürzten Reihe von Eigenschaften zurückgibt. Diese beibehaltene Abfrage treibt die Erlebnisliste der ersten Ansicht an.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` persistente Abfrage, die ein einzelnes Abenteuer von `slug` (eine benutzerdefinierte Eigenschaft, die ein Abenteuer eindeutig identifiziert) mit einer vollständigen Reihe von Eigenschaften. Diese beibehaltene Abfrage ermöglicht die Detailansicht des Abenteuers.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
      }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### Durchführen einer von GraphQL beibehaltenen Abfrage

AEM persistente Abfragen werden über HTTP-GET ausgeführt und daher die [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js) wird verwendet, um [die gespeicherten GraphQL-Abfragen ausführen](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) AEM und laden Sie den Abenteuerinhalt in die App.

Jede persistente Abfrage weist eine entsprechende React-Antwort auf [useEffect](https://reactjs.org/docs/hooks-effect.html) Hook `src/api/usePersistedQueries.js`, der asynchron den AEM HTTP-GET persistenten Abfrageendpunkt aufruft und die Abenteuerdaten zurückgibt.

Jede Funktion ruft wiederum die `aemHeadlessClient.runPersistedQuery(...)`, um die persistente GraphQL-Abfrage auszuführen.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity) {
  ...
  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    const queryParameters = { activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryParameters);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all");
  }
  
  ... 
}

...
/**
 * Private function that invokes the AEM Headless client.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  return { data, err };
}
```

### Ansichten

Die React-Anwendung verwendet zwei Ansichten, um die Abenteuerdaten im Web-Erlebnis darzustellen.

+ `src/components/Adventures.js`

   Aufrufe `getAdventuresByActivity(..)` von `src/api/usePersistedQueries.js` und zeigt die zurückgegebenen Abenteuer in einer Liste an.

+ `src/components/AdventureDetail.js`

   Ruft die `getAdventureBySlug(..)` mithilfe der `slug` über die Abenteuerauswahl auf der `Adventures` und zeigt die Details eines einzelnen Abenteuers an.

### Umgebungsvariablen

Mehrere [Umgebungsvariablen](https://create-react-app.dev/docs/adding-custom-environment-variables) werden verwendet, um eine Verbindung zu einer AEM-Umgebung herzustellen. Standardmäßige Verbindungen zur AEM-Veröffentlichung, die unter ausgeführt wird `http://localhost:4503`. Aktualisieren Sie die `.env.development` Datei, um die AEM Verbindung zu ändern:

+ `REACT_APP_HOST_URI=http://localhost:4502`: Auf AEM Zielhost eingestellt
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: Legen Sie den GraphQL-Endpunktpfad fest. Dies wird von dieser React-App nicht verwendet, da diese App nur persistente Abfragen verwendet.
+ `REACT_APP_AUTH_METHOD=`: Die bevorzugte Authentifizierungsmethode. Optional, standardmäßig wird keine Authentifizierung verwendet.
   + `service-token`: Verwenden Sie Dienstanmeldeinformationen, um ein Zugriffstoken auf AEM as a Cloud Service abzurufen.
   + `dev-token`: Verwenden des Dev-Tokens für die lokale Entwicklung auf AEM as a Cloud Service
   + `basic`: Benutzer/Pass für lokale Entwicklung mit lokaler AEM-Autoreninstanz verwenden
   + Leer lassen, um ohne Authentifizierung eine Verbindung zu AEM
+ `REACT_APP_AUTHORIZATION=admin:admin`: Legen Sie grundlegende Authentifizierungsberechtigungen fest, die beim Herstellen einer Verbindung zu einer AEM-Autorenumgebung verwendet werden (nur für die Entwicklung). Wenn Sie eine Verbindung zu einer Veröffentlichungsumgebung herstellen, ist diese Einstellung nicht erforderlich.
+ `REACT_APP_DEV_TOKEN`: Entwicklungstoken-Zeichenfolge. Um eine Verbindung zur Remote-Instanz herzustellen, können Sie neben der einfachen Authentifizierung (user:pass) die Bearer-Authentifizierung mit DEV-Token über die Cloud-Konsole verwenden
+ `REACT_APP_SERVICE_TOKEN`: Pfad zur Datei mit Dienstanmeldeinformationen. Um eine Verbindung zur Remote-Instanz herzustellen, kann die Authentifizierung auch über das Service-Token erfolgen (Download-Datei über die Developer Console).

### Proxy-AEM

Bei Verwendung des Webpack Development Servers (`npm start`) das Projekt auf eine [Proxy-Einrichtung](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware`. Die Datei wird konfiguriert unter [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) und verwendet mehrere benutzerdefinierte Umgebungsvariablen, die auf `.env` und `.env.development`.

Wenn eine Verbindung zu einer AEM Autorenumgebung hergestellt wird, muss die entsprechende [Authentifizierungsmethode muss konfiguriert werden](#environment-variables).

### Cross-Origin Resource Sharing (CORS)

Diese React-Anwendung basiert auf einer AEM-basierten CORS-Konfiguration, die in der AEM-Zielumgebung ausgeführt wird, und geht davon aus, dass die React-App auf `http://localhost:3000` im Entwicklungsmodus. Die [CORS-Konfiguration](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) ist Teil der [WKND-Site](https://github.com/adobe/aem-guides-wknd).

![CORS-Konfiguration](assets/react-app/cross-origin-resource-sharing-configuration.png)
