---
title: Node.js-Server-zu-Server-App – AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Server-seitige Node.js-Anwendung zeigt, wie Inhalte mithilfe von AEM GraphQL-APIs unter Verwendung persistierter Abfragen abgerufen werden können.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
duration: 172
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 100%

---

# Node.js-Server-zu-Server-App

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Server-zu-Server-Anwendung zeigt, wie Inhalte mithilfe von AEM GraphQL-APIs unter Verwendung persistierter Abfragen abgerufen und auf einem Terminal gedruckt werden können.

![Node.js-Server-zu-Server-App mit AEM Headless](./assets/server-to-server-app/server-to-server-app.png)

Sie finden den [Quell-Code auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server).

## Voraussetzungen {#prerequisites}

Folgende Tools sollten lokal installiert werden:

+ [Node.js v18](https://nodejs.org/de)
+ [Git](https://git-scm.com/)

## AEM-Anforderungen

Die Node.js-Anwendung funktioniert mit den folgenden AEM-Bereitstellungsoptionen. Für alle Bereitstellungen muss die [WKND-Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) installiert werden.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=de)
+ Optional [Service-Anmeldeinformationen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=de) bei Autorisierung von Anfragen (z. B. Verbindung mit dem AEM-Autoren-Service)

Diese Node.js-Anwendung kann basierend auf den Befehlszeilenparametern eine Verbindung zu AEM Author oder AEM Publish herstellen.

## Informationen zur Verwendung

1. Klonen des Repositorys `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Öffnen Sie ein Terminal und führen Sie die folgenden Befehle aus:

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. Die App kann mit dem folgenden Befehl ausgeführt werden:

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   So führen Sie beispielsweise die App für AEM Publish ohne Autorisierung aus:

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   So führen Sie die App für AEM Author mit Autorisierung aus:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Eine JSON-Liste mit Adventures von der WKND-Referenz-Website sollte auf dem Terminal gedruckt werden.

## Der Code

Nachstehend finden Sie eine Zusammenfassung dazu, wie die Node.js-Server-zu-Server-Anwendung erstellt wird, wie eine Verbindung mit AEM Headless hergestellt wird, um Inhalte mithilfe persistierter GraphQL-Abfragen abzurufen, und wie diese Daten dargestellt werden. Den vollständigen Code finden Sie auf [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server).

Der gängige Anwendungsfall für AEM Headless-Server-zu-Server-Apps besteht darin, Inhaltsfragmentdaten von AEM mit anderen Systemen zu synchronisieren. Diese Anwendung ist jedoch absichtlich einfach gehalten und druckt die JSON-Ergebnisse aus der persistenten Abfrage.

### Persistierte Abfragen

Gemäß den Best Practices für AEM Headless verwendet die Anwendung AEM GraphQL-persistierte Abfragen, um Adventure-Daten abzufragen. Die Anwendung verwendet zwei persistierte Abfragen:

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

### Erstellen eines AEM Headless-Clients

```javascript
const { AEMHeadless, getToken } = require('@adobe/aem-headless-client-nodejs');

async function run() { 

    // Parse the AEM host, and optional service credentials from the command line arguments
    const args = process.argv.slice(2);
    const aemHost = args.length > 0 ? args[0] : null;                // Example: https://author-p123-e456.adobeaemcloud.com
    const serviceCredentialsFile = args.length > 1 ? args[1] : null; // Example: ./service-config.json

    // If service credentials are provided via command line argument,
    // use `getToken(..)` to exchange them with Adobe IMS for an AEM access token 
    let accessToken;
    if (serviceCredentialsFile) {
        accessToken = (await getToken(serviceCredentialsFile)).accessToken;
    }

    // Instantiate withe AEM Headless client to query AEM GraphQL APIs
    // The endpoint is left blank since only persisted queries should be used to query AEM's GraphQL APIs
    const aemHeadlessClient = new AEMHeadless({
        serviceURL: aemHost,
        endpoint: '',           // Avoid non-persisted queries
        auth: accessToken       // accessToken only set if the 2nd command line parameter is set
    })
    ...
}
```


### Durchführen einer GraphQL-persistierten Abfrage

Persistierte AEM-Abfragen werden über HTTP-GET ausgeführt. Daher wird der [AEM Headless-Client für Node.js](https://github.com/adobe/aem-headless-client-nodejs) verwendet, um [persistierte GraphQL-Abfragen für AEM auszuführen](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) und so Adventure-inhalte abzurufen.

Die persistierte Abfrage wird durch Aufruf von `aemHeadlessClient.runPersistedQuery(...)` und Übergabe des Namens der persistenten GraphQL-Abfrage aufgerufen. Sobald GraphQL die Daten zurückgibt, übergeben Sie diese an die vereinfachte Funktion `doSomethingWithDataFromAEM(..)`, die die Ergebnisse druckt. Normalerweise würden die Daten jedoch an ein anderes System gesendet oder basierend auf den abgerufenen Daten würde eine Ausgabe generiert werden.

```js
// index.js

async function run() { 
    ...
    try {
        // Retrieve the data from AEM GraphQL APIs
        data = await aemHeadlessClient.runPersistedQuery('wknd-shared/adventures-all')
        
        // Do something with the data from AEM. 
        // A common use case is sending the data to another system.
        await doSomethingWithDataFromAEM(data);
    } catch (e) {
        console.error(e.toJSON())
    }
}
```
