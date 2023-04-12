---
title: Verwendung des AEM Headless-SDK
description: Erfahren Sie, wie Sie GraphQL-Abfragen mit dem AEM Headless-SDK durchführen.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 595d990b7d8ed3c801a085892fef38d780082a15
workflow-type: ht
source-wordcount: '415'
ht-degree: 100%

---

# AEM Headless SDK

Das AEM Headless-SDK ist eine Reihe von Bibliotheken, die von Clients verwendet werden können, um schnell und einfach mit AEM Headless-APIs über HTTP zu interagieren.

Das AEM Headless-SDK ist für verschiedene Plattformen verfügbar:

+ [AEM Headless-SDK für Client-seitige Browser (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM Headless-SDK für Server-seitig/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM Headless-SDK für Java™](https://github.com/adobe/aem-headless-client-java)

## Persistierte GraphQL-Abfragen

Das Abfragen von AEM mit GraphQL unter Verwendung von persistierten Abfragen (im Gegensatz zu [Client-definierten GraphQL-Abfragen](#graphl-queries)) ermöglicht es Entwicklerinnen und Entwicklern, eine Abfrage (aber nicht deren Ergebnisse) in AEM zu persistieren und dann anzufordern, dass die Abfrage nach Namen ausgeführt wird. Persistierte Abfragen ähneln dem Konzept gespeicherter Prozeduren in SQL-Datenbanken.

Persistierte Abfragen sind leistungsfähiger als Client-definierte GraphQL-Abfragen, da persistierte Abfragen mithilfe von HTTP-GET ausgeführt werden, der auf den CDN- und AEM Dispatcher-Ebenen zwischenspeicherbar ist. Persistierte Abfragen sind auch effektiv, definieren eine API und entbinden die Entwicklerin bzw. den Entwickler von der Notwendigkeit, die Details der einzelnen Inhaltsfragmentmodelle zu verstehen.

### Code-Beispiele{#persisted-graphql-queries-code-examples}

Im Folgenden finden Sie Code-Beispiele zum Ausführen einer von GraphQL persistierten Abfrage gegen AEM.

+++ JavaScript-Beispiel

Installieren Sie [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) durch Ausführen des Befehls `npm install` aus dem Stammverzeichnis Ihres Node.js-Projekts.

```
$ npm i @adobe/aem-headless-client-js
```

Dieses Code-Beispiel zeigt, wie Sie AEM mit dem npm-Modul [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) unter Verwendung der Syntax `async/await` abfragen. Das AEM Headless-SDK für JavaScript unterstützt auch die [Promise-Syntax](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Dieser Code setzt eine persistierte Abfrage mit dem Namen `wknd/adventureNames` voraus, wurde in AEM Author erstellt und in AEM Publish veröffentlicht.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json',  // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Uses the AEM Headless SDK to execute a persisted query with optional query variables.

 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
export async function executePersistedQuery(persistedQueryName, queryParameters) {
    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName, queryParameters);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
        errors = e;
    }

    return { data, errors };
};

// Execute the persisted query using its name 'wknd-shared/adventures-by-slug' and optional query variables
let { data, errors } = executePersistedQuery('wknd-shared/adventures-by-slug', { "slug": "bali-surf-camp" });
```

+++

+++ React useEffect(..) Beispiel

Installieren Sie [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) durch Ausführen des Befehls `npm install` aus dem Stammverzeichnis Ihres React-Projekts.

```
$ npm i @adobe/aem-headless-client-js
```

In diesem Code-Beispiel wird gezeigt, wie der Hook [React useEffect(..) ](https://reactjs.org/docs/hooks-effect.html) verwendet wird, um einen asynchronen Aufruf an AEM GraphQL auszuführen.

Die Verwendung von `useEffect` für den asynchronen GraphQL-Aufruf in React ist aus folgenden Gründen nützlich:

1. Es stellt einen synchronen Wrapper für den asynchronen Aufruf an AEM bereit.
1. Es reduziert unnötige Neuanforderungen von AEM.

Dieser Code setzt eine persistierte Abfrage mit dem Namen `wknd-shared/adventure-by-slug` voraus, wurde in AEM Author erstellt und mit GraphiQL in AEM Publish veröffentlicht.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';
import { useEffect, useState } from "react";

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json'         // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Private, shared function that invokes the AEM Headless client. 
 * React components/views will invoke GraphQL via the custom React useEffect hooks defined below.
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

/**
 * Calls the 'wknd-shared/adventure-by-slug' and provided the {slug} as the persisted query's `slug` parameter.
 *
 * @param {String!} slug the unique slug used to specify the adventure to return
 * @returns a JSON object representing the adventure
 */
export function useAdventureBySlug(slug) {
  const [adventure, setAdventure] = useState(null);
  const [errors, setErrors] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // The key is the variable name as defined in the persisted query, and may not match the model's field name
      const queryParameters = { slug: slug };
      
      // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
      const { data, err } = await fetchPersistedQuery(
        "wknd-shared/adventure-by-slug",
        queryParameters
      );

      if (err) {
        // Capture errors from the HTTP request
        setErrors(err);
      } else if (data?.adventureList?.items?.length === 1) {
        // Set the adventure data after data validation (there should only be 1 matching adventure)
        setAdventure(data.adventureList.items[0]);
      } else {
        // Set an error if no adventure could be found
        setErrors(`Cannot find adventure with slug: ${slug}`);
      }
    }
    fetchData();
  }, [slug]);

  return { adventure, errors };
}
```

Rufen Sie den benutzerdefinierten React-Hook `useEffect` von einer anderen Stelle in einer React-Komponente auf.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

Es können neue `useEffect`-Hooks für jede persistierte Abfrage erstellt werden, die die React-App verwendet.

+++

<p> </p>

## GraphQL-Abfragen

AEM unterstützt Client-definierte GraphQL-Abfragen. Die beste AEM-Praxis ist jedoch, [persistierte GraphQL-Abfragen](#persisted-graphql-queries) zu verwenden.

