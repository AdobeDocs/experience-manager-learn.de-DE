---
title: Next.js – AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Next.js-App zeigt, wie Inhalte mit den AEM GraphQL-APIs unter Verwendung persistierter Abfragen abgefragt werden können.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
source-git-commit: 7938325427b6becb38ac230a3bc4b031353ca8b1
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 93%

---

# Next.js-App

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Next.js-App zeigt, wie Inhalte mit den AEM GraphQL-APIs unter Verwendung persistierter Abfragen abgefragt werden können. Der AEM Headless-Client für JavaScript wird verwendet, um die von GraphQL gespeicherten Abfragen auszuführen, die die App antreiben.

![Next.js-App mit AEM Headless](./assets/next-js/next-js.png)

Sie finden den [Quell-Code auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

## Voraussetzungen {#prerequisites}

Folgende Tools sollten lokal installiert werden:

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## AEM-Anforderungen

Die Next.js-App funktioniert mit den folgenden AEM-Bereitstellungsoptionen. Alle Implementierungen erfordern [WKND Shared v3.0.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) oder [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) in der AEM as a Cloud Service Umgebung installiert werden.

Diese Next.js-Beispiel-App ist für die Verbindung mit dem __AEM Publish__-Service konzipiert.

### AEM Author-Anforderungen

Next.js wurde entwickelt, um eine Verbindung zum __AEM Publish__-Service herzustellen und auf ungeschützte Inhalte zuzugreifen. Next.js kann so konfiguriert werden, dass eine Verbindung zu AEM Author über die unten beschriebenen `.env`-Eigenschaften hergestellt wird. Bilder, die von AEM Author bereitgestellt werden, erfordern eine Authentifizierung. Daher muss der Benutzende, der auf die Next.js-App zugreift, auch bei AEM Author angemeldet sein.

## Informationen zur Verwendung

1. Klonen Sie das Repository `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bearbeiten Sie die Datei `aem-guides-wknd-graphql/next-js/.env.local` und setzen Sie `NEXT_PUBLIC_AEM_HOST` auf den AEM-Service.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Wenn eine Verbindung zum AEM-Autoren-Service hergestellt wird, muss die Authentifizierung durchgeführt werden, da der AEM-Autoren-Service standardmäßig sicher ist.

   Um ein lokales AEM-Konto zu verwenden, setzen Sie `AEM_AUTH_METHOD=basic` und geben Sie den Benutzernamen und das Passwort in den Eigenschaften `AEM_AUTH_USER` und `AEM_AUTH_PASSWORD` an.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Um ein [lokales Entwicklungstoken von AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=de#generating-the-access-token) zu verwenden, setzen Sie `AEM_AUTH_METHOD=dev-token` und geben Sie den vollständigen Wert des Dev-Token in der Eigenschaft `AEM_AUTH_DEV_TOKEN` an.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Bearbeiten Sie die Datei `aem-guides-wknd-graphql/next-js/.env.local` und überprüfen Sie, ob `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` auf den entsprechenden AEM GraphQL-Endpunkt eingestellt ist.

   Wenn Sie [WKND Shared](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) oder [WKND Site](https://github.com/adobe/aem-guides-wknd/releases/latest) verwenden, benutzen Sie den `wknd-shared` GraphQL-API-Endpunkt.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. Öffnen Sie eine Eingabeaufforderung und starten Sie die Next.js-App mit den folgenden Befehlen:

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. Ein neues Browser-Fenster öffnet die Next.js-App unter [http://localhost:3000](http://localhost:3000)
1. Die Next.js-App zeigt eine Liste von Adventures an. Die Auswahl eines Adventures öffnet seine Details auf einer neuen Seite.

## Der Code

Nachstehend finden Sie eine Zusammenfassung dazu, wie die Next.js-App erstellt wurde, wie sie eine Verbindung zu AEM Headless herstellt, um mithilfe von durch GraphQL persistierten Abfragen Inhalte abzurufen und wie diese Daten dargestellt werden. Den vollständigen Code finden Sie auf [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Persistierte Abfragen

Gemäß den Best Practices von AEM Headless verwendet die Next.js-App AEM GraphQL persistierte Abfragen, um Adventure-Daten abzufragen. Die App verwendet zwei persistierte Abfragen:

+ `wknd/adventures-all` persistierte Abfrage, die alle Adventures in AEM mit einer gekürzten Reihe von Eigenschaften zurückgibt. Diese persistierte Abfrage bestimmt die Erlebnisliste der ersten Ansicht.

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

Die persistierten Abfragen von AEM werden über HTTP GET ausgeführt und daher wird der [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js) verwendet, um [die persistierten GraphQL-Abfragen](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) gegen AEM auszuführen und den Adventure-Inhalt in die App zu laden.

Jede persistierte Abfrage verfügt über eine entsprechende Funktion in `src/lib//aem-headless-client.js`, die den AEM GraphQL-Endpunkt aufruft und die Adventure-Daten zurückgibt.

Jede Funktion ruft wiederum die `aemHeadlessClient.runPersistedQuery(...)` auf, um die persistierte GraphQL-Abfrage auszuführen.

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

async getAdventureSlugs(queryVariables) { ... }

async getAdventuresBySlug(slug, queryVariables) { ... }
...
```

### Seiten

Die Next.js-App verwendet zwei Seiten, um die Adventure-Daten darzustellen.

+ `src/pages/index.js`

  Verwendet [getServerSideProps() von Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props), um `getAllAdventures()` aufzurufen und zeigt jedes Adventure als Karte an.

  Die Verwendung von `getServerSiteProps()` ermöglicht das serverseitige Rendering dieser Next.js-Seite.

+ `src/pages/adventures/[...slug].js`

  Eine [Dynamische Route von Next.js](https://nextjs.org/docs/routing/dynamic-routes), die die Details eines einzelnen Adventures anzeigt. Diese dynamische Route ruft die Daten jedes Abenteuers mithilfe von [getStaticProps() von Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) über einen Aufruf an `getAdventureBySlug(slug, queryVariables)` mithilfe der `slug` Param, der über die Abenteuerauswahl auf der `adventures/index.js` und `queryVariables` um das Bildformat, die Breite und die Qualität zu steuern.

  Die dynamische Route ist in der Lage, die Details für alle Adventures im Voraus abzurufen, indem sie [getStaticPaths() von Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) verwendet und alle möglichen Routen-Permutationen auf der Grundlage der vollständigen Liste der Adventures, die von der GraphQL-Abfrage `getAdventurePaths()` zurückgegeben werden, auffüllt.

  Die Verwendung von `getStaticPaths()` und `getStaticProps(..)` ermöglichte die statische Site-Generierung dieser Next.js-Seiten.

## Bereitstellungskonfiguration

Next.js-Apps, insbesondere im Zusammenhang mit Server-seitigem Rendering (SSR) und Server-seitiger Generierung (SSG), erfordern keine erweiterten Sicherheitskonfigurationen wie Cross-Origin Resource Sharing (CORS).

Wenn jedoch Next.js im Client-Kontext HTTP-Anfragen an AEM sendet, sind möglicherweise Sicherheitskonfigurationen in AEM erforderlich. Überprüfen Sie das [Tutorial zur Bereitstellung von AEM Headless-Einzelseiten-App](../deployment/spa.md) für weitere Details.
