---
title: React-App – AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese React-App zeigt, wie Sie mithilfe von AEM GraphQL-APIs unter Verwendung von persistierten Abfragen Inhalte abfragen können.
version: Cloud Service
mini-toc-levels: 1
jira: KT-10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
duration: 272
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 100%

---

# React-App{#react-app}

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese React-App zeigt, wie Sie mithilfe von AEM GraphQL-APIs unter Verwendung von persistierten Abfragen Inhalte abfragen können. Der AEM Headless-Client für JavaScript wird verwendet, um die von GraphQL gespeicherten Abfragen auszuführen, die die App antreiben.

![React-App mit AEM Headless](./assets/react-app/react-app.png)

Sie finden den [Quell-Code auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

Es ist ein [vollständiges Schritt-für-Schritt-Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=de) verfügbar, in dem beschrieben wird, wie diese React-App erstellt wurde.

## Voraussetzungen {#prerequisites}

Folgende Tools sollten lokal installiert werden:

+ [Node.js v18](https://nodejs.org/de/)
+ [Git](https://git-scm.com/)

## AEM-Anforderungen

Die React-App funktioniert mit den folgenden AEM-Bereitstellungsoptionen. Für alle Bereitstellungen muss die [WKND-Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) installiert werden.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=de)
+ Lokales Setup mit dem [AEM Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de)
   + Erfordert [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)

Die React-App ist für die Verbindung mit einer __AEM Publish__-Umgebung vorgesehen, kann jedoch Inhalte von AEM Author beziehen, wenn die Authentifizierung in der Konfiguration der React-App bereitgestellt wird.

## Informationen zur Verwendung

1. Klonen Sie das Repository `adobe/aem-guides-wknd-graphql`. 

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bearbeiten Sie die Datei `aem-guides-wknd-graphql/react-app/.env.development` und richten Sie den `REACT_APP_HOST_URI` so ein, dass er auf Ihre AEM verweist.

   Aktualisieren Sie die Authentifizierungsmethode, wenn Sie eine Verbindung zu einer Autoreninstanz herstellen.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
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

1. Öffnen Sie ein Terminal und führen Sie die folgenden Befehle aus:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Ein neues Browser-Fenster sollte in [http://localhost:3000](http://localhost:3000) geladen werden.
1. Eine Liste der Adventures von der WKND-Referenz-Website sollte auf der App angezeigt werden.

## Der Code

Nachstehend finden Sie eine Zusammenfassung dazu, wie die React-App aufgebaut ist, wie sie eine Verbindung zu AEM Headless herstellt, um mithilfe von persistierten Abfragen von GraphQL Inhalte abzurufen, und wie diese Daten dargestellt werden. Den vollständigen Code finden Sie auf [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Persistierte Abfragen

Gemäß den Best Practices für AEM verwendet die React-App persistierte Anfragen von AEM GraphQL, um Adventure-Daten abzufragen. Die Anwendung verwendet zwei persistierte Abfragen:

+ Die persistierte Abfrage `wknd/adventures-all` gibt alle Adventures in AEM mit einer gekürzten Reihe von Eigenschaften zurück. Diese persistierte Abfrage bestimmt die Erlebnisliste der ersten Ansicht.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ Die persistierte Abfrage `wknd/adventure-by-slug` gibt ein einzelnes Adventure durch `slug` (eine benutzerdefinierte Eigenschaft, die ein Adventure eindeutig identifiziert) mit einer vollständigen Reihe von Eigenschaften zurück. Diese persistierte Abfrage ermöglicht Detailansichten des Adventures.

```
# Retrieves an Adventure Fragment based on it's unique slug.
#
# Required query variables:
# - {"slug": "bali-surf-camp"}
#
# Optional query variables:
# - { 
#     "imageFormat": "JPG",
#     "imageSeoName": "my-adventure",
#     "imageWidth": 1600,
#     "imageQuality": 90 
#   }
#  
# This query returns an adventure list but since the the slug property is set to be unique in the Content Fragment Model, only a single Content Fragment is expected.

query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
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
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
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

### Durchführen einer GraphQL-persistierten Abfrage

Die persistierten Abfragen von AEM werden über HTTP-GET ausgeführt, und daher wird der [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js) verwendet, um [die persistierten GraphQL-Abfragen an AEM auszuführen](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) und den Adventure-Inhalt in die App zu laden.

Jede persistierte Abfrage hat einen entsprechenden React-[useEffect](https://reactjs.org/docs/hooks-effect.html)-Hook in `src/api/usePersistedQueries.js`, der asynchron den Endpunkt der asynchronen Abfrage von AEM HTTP-GET abruft und die Adventure-Daten zurückgibt.

Jede Funktion ruft wiederum `aemHeadlessClient.runPersistedQuery(...)` auf, um die persistierte GraphQL-Abfrage auszuführen.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity, params = {}) {
  ...
  let queryVariables = params;

  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    queryVariables = { ...queryVariables, activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryVariables);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all", queryVariables);
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

Die React-App verwendet zwei Ansichten, um die Adventure-Daten im Web-Erlebnis darzustellen.

+ `src/components/Adventures.js`

  Ruft `getAdventuresByActivity(..)` von `src/api/usePersistedQueries.js` auf und zeigt die zurückgegebenen Adventures in einer Liste an.

+ `src/components/AdventureDetail.js`

  Ruft `getAdventureBySlug(..)` mithilfe des Parameters `slug` über die Adventure-Auswahl auf der `Adventures`-Komponente auf und zeigt die Details eines einzelnen Adventures an.

### Umgebungsvariablen

Mehrere [Umgebungsvariablen](https://create-react-app.dev/docs/adding-custom-environment-variables) werden verwendet, um eine Verbindung zu einer AEM-Umgebung herzustellen. Standardmäßige Verbindungen zu AEM Publish, das unter `http://localhost:4503` ausgeführt wird. Aktualisieren Sie die Datei `.env.development`, um die AEM-Verbindung zu ändern:

+ `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`: Auf AEM-Ziel-Host einstellen
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: Den GraphQL-Endpunktpfad festlegen. Dies wird von dieser React-App nicht verwendet, da diese App nur persistierte Abfragen verwendet.
+ `REACT_APP_AUTH_METHOD=`: Die bevorzugte Authentifizierungsmethode. Optional, da standardmäßig keine Authentifizierung verwendet wird.
   + `service-token`: Verwenden von Service-Anmeldeinformationen, um ein Zugriffstoken für AEM as a Cloud Service zu erhalten.
   + `dev-token`: Verwenden des Entwicklungs-Tokens für die lokale Entwicklung auf AEM as a Cloud Service
   + `basic`: Verwenden von Benutzernamen/Kennwort für lokale Entwicklung mit lokaler AEM-Autoreninstanz
   + Leer lassen, um ohne Authentifizierung eine Verbindung zu AEM herzustellen
+ `REACT_APP_AUTHORIZATION=admin:admin`: Festlegen der grundlegenden Authentifizierungs-Anmeldeinformationen, die beim Herstellen einer Verbindung zu einer AEM Author-Umgebung verwendet werden (nur für die Entwicklung). Wenn Sie eine Verbindung zu einer Veröffentlichungsumgebung herstellen, ist diese Einstellung nicht erforderlich.
+ `REACT_APP_DEV_TOKEN`: Entwicklungs-Token-Zeichenfolge. Um eine Verbindung zu einer Remote-Instanz herzustellen, können Sie neben der einfachen Authentifizierung (user:pass) auch die Bearer-Authentifizierung mit dem Entwicklungs-Token über die Cloud-Konsole verwenden
+ `REACT_APP_SERVICE_TOKEN`: Pfad zur Datei mit den Service-Anmeldeinformationen. Um eine Verbindung zu einer Remote-Instanz herzustellen, kann die Authentifizierung auch über das Service-Token erfolgen (laden Sie die Datei über die Developer Console herunter).

### Proxy-AEM-Abfragen

Bei Verwendung des Webpack Development Servers (`npm start`) verlässt sich das Projekt auf eine [Proxy-Einrichtung](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) unter Verwendung von `http-proxy-middleware`. Die Datei wird konfiguriert unter [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) und verwendet mehrere benutzerdefinierte Umgebungsvariablen, die in `.env` und `.env.development` festgelegt werden.

Wenn eine Verbindung zu einer AEM Author-Umgebung hergestellt wird, muss die entsprechende [Authentifizierungsmethode konfiguriert werden](#environment-variables).

### Cross-Origin Resource Sharing (CORS)

Diese React-App basiert auf einer AEM-basierten CORS-Konfiguration, die in der AEM-Zielumgebung ausgeführt wird, und es wird davon ausgegangen, dass die React-App auf `http://localhost:3000` im Entwicklungsmodus ausgeführt wird.  Weitere Informationen zum Einrichten und Konfigurieren von CORS finden Sie in der [Dokumentation zur AEM Headless-Implementierung](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa.html?lang=de).
