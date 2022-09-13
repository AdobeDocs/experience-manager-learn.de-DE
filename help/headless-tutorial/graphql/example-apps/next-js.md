---
title: Next.js - AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Next.js-Anwendung zeigt, wie Sie Inhalte mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen abfragen können.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
source-git-commit: b2bf2a8e454d7ccd09819f2a38e58f7c303cb066
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 2%

---

# Next.js-App

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Next.js-Anwendung zeigt, wie Sie Inhalte mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen abfragen können. Der AEM Headless-Client für JavaScript wird verwendet, um die von GraphQL gespeicherten Abfragen auszuführen, die die App unterstützen.

![Next.js-App mit AEM Headless](./assets/next-js/next-js.png)

Anzeigen der [Quellcode auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Voraussetzungen {#prerequisites}

Die folgenden Tools sollten lokal installiert werden:

+ [Node.js v16+](https://nodejs.org/en/)
+ [npm 8+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM

Das Programm Next.js funktioniert mit den folgenden AEM Bereitstellungsoptionen. Alle Implementierungen erfordern [WKND Shared v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest), [WKND Site v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)oder [Referenz-Demo-Add-on](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html) in der AEM as a Cloud Service Umgebung installiert werden.

Diese Beispielanwendung &quot;Next.js&quot;dient dazu, eine Verbindung zu __AEM-Veröffentlichung__ Dienst.

### AEM-Autorenanforderungen

Next.js ist für die Verbindung mit __AEM-Veröffentlichung__ und auf nicht geschützte Inhalte zugreifen. Next.js kann so konfiguriert werden, dass eine Verbindung zur AEM-Autoreninstanz über die `.env` Eigenschaften, die unten beschrieben sind. Bilder, die von der AEM-Autoreninstanz bereitgestellt werden, erfordern eine Authentifizierung. Daher muss der Benutzer, der auf die Next.js-App zugreift, auch bei der AEM-Autoreninstanz angemeldet sein.

## Informationen zur Verwendung

1. Klonen Sie die `adobe/aem-guides-wknd-graphql` repository:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bearbeiten Sie die `aem-guides-wknd-graphql/next-js/.env.local` Datei und Set `NEXT_PUBLIC_AEM_HOST` zum AEM Dienst.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Wenn eine Verbindung zum AEM-Autorendienst hergestellt wird, muss die Authentifizierung durchgeführt werden, da der AEM-Autorendienst standardmäßig sicher ist.

   So verwenden Sie einen lokalen AEM `AEM_AUTH_METHOD=basic` und geben Sie den Benutzernamen und das Kennwort im `AEM_AUTH_USER` und `AEM_AUTH_PASSWORD` Eigenschaften.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   So verwenden Sie eine [as a Cloud Service Token für die lokale Entwicklung AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` und geben Sie den vollständigen Dev-Token-Wert im `AEM_AUTH_DEV_TOKEN` -Eigenschaft.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Bearbeiten Sie die `aem-guides-wknd-graphql/next-js/.env.local` Datei und Validierung  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` auf den entsprechenden GraphQL-Endpunkt AEM.

   Bei Verwendung von [WKND Shared](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) oder [WKND-Site](https://github.com/adobe/aem-guides-wknd/releases/latest), verwenden Sie die `wknd-shared` GraphQL-API-Endpunkt.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

   Bei Verwendung von [Referenz-Demo-Add-on](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html), verwenden Sie die `aem-demo-assets` GraphQL-API-Endpunkt.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=aem-demo-assets
   ...
   ```

1. Öffnen Sie eine Eingabeaufforderung und starten Sie die App Next.js mit den folgenden Befehlen:

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. Ein neues Browser-Fenster öffnet die App Next.js unter [http://localhost:3000](http://localhost:3000)
1. Die Next.js-App zeigt eine Liste von Abenteuern an. Die Auswahl eines Abenteuers öffnet seine Details auf einer neuen Seite.

## Der Code

Nachstehend finden Sie eine Zusammenfassung dazu, wie die Next.js-App erstellt wurde, wie sie eine Verbindung zu AEM Headless herstellt, um Inhalte mithilfe von GraphQL-gespeicherten Abfragen abzurufen, und wie diese Daten dargestellt werden. Den vollständigen Code finden Sie unter [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Beständige Abfragen

Gemäß AEM Best Practices für Headless verwendet die Next.js-App AEM von GraphQL gespeicherten Abfragen, um Abenteuerdaten abzufragen. Das Programm verwendet zwei persistente Abfragen:

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

### GraphQL-persistente Abfrage ausführen

AEM persistente Abfragen werden über HTTP-GET ausgeführt und daher die [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js) wird verwendet, um [die beibehaltenen GraphQL-Abfragen ausführen](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) AEM und laden Sie den Abenteuerinhalt in die App.

Jede persistente Abfrage verfügt über eine entsprechende Funktion in `src/lib//aem-headless-client.js`, der den AEM GraphQL-Endpunkt aufruft und die Abenteuerdaten zurückgibt.

Jede Funktion ruft wiederum die `aemHeadlessClient.runPersistedQuery(...)`, um die beibehaltene GraphQL-Abfrage auszuführen.

```js
// src/lib/aem-headless-client.js

...
/**
 * Invokes the 'adventures-all` persisted query using the parameterizable namespace.
 * 
 * @returns a GraphQL response of all adventures.
 */
async getAllAdventures() {
  const queryAdventuresAll = process.env.NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT + '/adventures-all';
    
  try {
    return await this.aemHeadlessClient.runPersistedQuery(queryAdventuresAll);
  } catch(e) {
    console.error(e)
  }    
}

// And so on, and so forth ... 

async getAdventureSlugs() { ... }

async getAdventuresBySlug(slug) { ... }
...
```

### Seiten

Die Anwendung Next.js verwendet zwei Seiten, um die Abenteuerdaten darzustellen.

+ `src/pages/index.js`

   Verwendet [getServerSideProps() von Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) aufrufen `getAllAdventures()` und zeigt jedes Abenteuer als Karte an.

   Die Verwendung von `getServerSiteProps()` ermöglicht das serverseitige Rendering dieser &quot;Next.js&quot;-Seite.

+ `src/pages/adventures/[...slug].js`

   A [Dynamische Route von Next.js](https://nextjs.org/docs/routing/dynamic-routes) , das die Details eines einzelnen Abenteuers anzeigt. Diese dynamische Route ruft die Daten jedes Abenteuers mithilfe von [getStaticProps() von Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) über einen Aufruf an `getAdventureBySlug(..)` mithilfe der `slug` über die Abenteuerauswahl auf der `adventures/index.js` Seite.

   Die dynamische Route kann die Details für alle Abenteuer vorab abrufen, indem sie [getStaticPaths() von Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) und das Füllen aller möglichen Routenberechtigungen basierend auf der vollständigen Liste der von der GraphQL-Abfrage zurückgegebenen Abenteuer  `getAdventurePaths()`

   Die Verwendung von `getStaticPaths()` und `getStaticProps(..)` die statische Site-Erstellung dieser &quot;Next.js&quot;-Seiten zugelassen.

## Bereitstellungskonfiguration

Next.js-Apps, insbesondere im Zusammenhang mit Server-seitigem Rendering (SSR) und Server-seitiger Generierung (SSG), erfordern keine erweiterten Sicherheitskonfigurationen wie Cross-Origin Resource Sharing (CORS).

Wenn jedoch Next.js im Client-Kontext HTTP-Anfragen an AEM sendet, sind möglicherweise Sicherheitskonfigurationen in AEM erforderlich. Überprüfen Sie die [Tutorial zur Implementierung von AEM Headless-Single-Page-Apps](../deployment/spa.md) für weitere Details.

