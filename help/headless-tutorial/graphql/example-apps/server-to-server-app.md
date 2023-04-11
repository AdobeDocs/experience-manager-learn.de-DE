---
title: Node.js-App von Server zu Server - AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese serverseitige Node.js-Anwendung zeigt, wie Inhalte mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen abgefragt werden können.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10798
thumbnail: KT-10798.jpg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 6%

---

# Node.js-App &quot;Server-zu-Server&quot;

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Server-zu-Server-Anwendung zeigt, wie Inhalte mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen abgerufen und auf dem Terminal gedruckt werden.

![Node.js-App von Server zu Server mit AEM Headless](./assets/server-to-server-app/server-to-server-app.png)

Anzeigen der [Quellcode auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app)

## Voraussetzungen {#prerequisites}

Die folgenden Tools sollten lokal installiert werden:

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM

Die Node.js-Anwendung funktioniert mit den folgenden AEM Bereitstellungsoptionen. Für alle Implementierungen ist die [WKND Site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) installiert werden.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=de)
+ Optional, [Service-Anmeldeinformationen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) wenn Anforderungen autorisiert werden (z. B. Verbindung zum AEM-Autorendienst).

Diese Node.js-Anwendung kann basierend auf den Befehlszeilenparametern eine Verbindung zur AEM-Autoren- oder AEM-Veröffentlichungsinstanz herstellen.

## Informationen zur Verwendung

1. Klonen Sie die `adobe/aem-guides-wknd-graphql` repository:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Öffnen Sie ein Terminal und führen Sie die Befehle aus:

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

   So führen Sie die App mit der Autorisierung für AEM Author aus:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Eine JSON-Liste mit Abenteuern von der WKND-Referenz-Website sollte im Terminal gedruckt werden.

## Der Code

Nachstehend finden Sie eine Zusammenfassung dazu, wie die Node.js-Anwendung &quot;Server-zu-Server&quot;erstellt wurde, wie eine Verbindung mit AEM Headless hergestellt wird, um Inhalte mithilfe von durch GraphQL gespeicherten Abfragen abzurufen, und wie diese Daten dargestellt werden. Den vollständigen Code finden Sie unter [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app).

Der gängige Anwendungsfall für Server-zu-Server-AEM Headless-Apps besteht darin, Inhaltsfragmentdaten von AEM in andere Systeme zu synchronisieren. Diese Anwendung ist jedoch absichtlich einfach und druckt die JSON-Ergebnisse aus der persistenten Abfrage.

### Beständige Abfragen

Gemäß AEM Best Practices für Headless verwendet die Anwendung AEM von GraphQL gespeicherten Abfragen, um Abenteuerdaten abzufragen. Die Anwendung verwendet zwei persistente Abfragen:

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

### Erstellen AEM Headless-Clients

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


### Durchführen einer von GraphQL beibehaltenen Abfrage

AEM persistente Abfragen werden über HTTP-GET ausgeführt und daher die [AEM Headless-Client für Node.js](https://github.com/adobe/aem-headless-client-nodejs) wird verwendet, um [die gespeicherten GraphQL-Abfragen ausführen](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) AEM und ruft den Inhalt des Abenteuers ab.

Die beibehaltene Abfrage wird durch Aufruf von `aemHeadlessClient.runPersistedQuery(...)`und übergeben Sie den Namen der gespeicherten GraphQL-Abfrage. Sobald die GraphQL die Daten zurückgibt, übergeben Sie sie an die vereinfachte `doSomethingWithDataFromAEM(..)` -Funktion, die die Ergebnisse druckt, aber normalerweise die Daten an ein anderes System sendet oder basierend auf den abgerufenen Daten eine Ausgabe generiert.

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
