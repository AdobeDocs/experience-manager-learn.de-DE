---
title: Verwenden des AEM Headless SDK
description: Erfahren Sie, wie Sie mit dem AEM Headless SDK GraphQL-Abfragen erstellen.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 68970493802c7194bcb3ac3ac9ee10dbfb0fc55d
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 1%

---

# AEM Headless-SDK

Das AEM Headless-SDK besteht aus Bibliotheken, die von Clients verwendet werden können, um schnell und einfach mit AEM Headless-APIs über HTTP zu interagieren.

Das AEM Headless-SDK ist für verschiedene Plattformen verfügbar:

+ [AEM Headless SDK für Client-seitige Browser (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM Headless SDK für server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM Headless-SDK für Java™](https://github.com/adobe/aem-headless-client-java)

## GraphQL-Abfragen

AEM unterstützt clientdefinierte GraphQL-Abfragen, es AEM jedoch Best Practice, [persistente GraphQL-Abfragen](#persisted-graphql-queries).

## Persistente GraphQL-Abfragen

Abfrage AEM GraphQL mit persistenten Abfragen (im Gegensatz zu [Client-definierte GraphQL-Abfragen](#graphl-queries)) ermöglicht es Entwicklern, eine Abfrage (aber nicht ihre Ergebnisse) in AEM beizubehalten und die Abfrage dann nach Namen auszuführen. Persistente Abfragen ähneln dem Konzept gespeicherter Prozeduren in SQL-Datenbanken.

Persistente Abfragen sind leistungsfähiger als clientdefinierte GraphQL-Abfragen, da persistente Abfragen über HTTP-GET ausgeführt werden, der auf den CDN- und AEM Dispatcher-Ebenen zwischenspeicherbar ist. Persistente Abfragen sind auch effektiv, definieren eine API und entkoppeln die Notwendigkeit, dass der Entwickler die Details der einzelnen Inhaltsfragmentmodelle versteht.

### Code-Beispiele{#persisted-graphql-queries-code-examples}

Im Folgenden finden Sie Codebeispiele zum Ausführen einer von GraphQL beibehaltenen Abfrage gegen AEM.

+++ JavaScript-Beispiel

Installieren Sie die [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) durch Ausführen der `npm install` aus dem Stammverzeichnis Ihres Node.js-Projekts.

```
$ npm i @adobe/aem-headless-client-js
```

Dieses Codebeispiel zeigt, wie AEM mithilfe der [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm-Modul mit `async/await` Syntax. Das AEM Headless-SDK für JavaScript unterstützt auch [Syntax von Promise](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Dieser Code setzt eine persistente Abfrage mit dem Namen voraus `wknd/adventureNames` wurde in der AEM-Autoreninstanz erstellt und in der AEM-Veröffentlichungsinstanz veröffentlicht.

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

Installieren Sie die [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) durch Ausführen der `npm install` aus dem Stammverzeichnis Ihres React-Projekts.

```
$ npm i @adobe/aem-headless-client-js
```

In diesem Codebeispiel wird gezeigt, wie die [React useEffect(..) Hook](https://reactjs.org/docs/hooks-effect.html) , um einen asynchronen Aufruf an AEM GraphQL auszuführen.

Verwenden `useEffect` , um den asynchronen GraphQL-Aufruf in React durchzuführen, ist aus folgenden Gründen nützlich:

1. Es stellt einen synchronen Wrapper für den asynchronen Aufruf an AEM bereit.
1. Dadurch wird unnötiges AEM reduziert.

Dieser Code setzt eine persistente Abfrage mit dem Namen voraus `wknd-shared/adventure-by-slug` wurde in der AEM-Autoreninstanz erstellt und mit GraphiQL in der AEM-Veröffentlichungsinstanz veröffentlicht.

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

Benutzerdefinierte React aufrufen `useEffect` -Hook von einer anderen Stelle in einer React-Komponente.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

Neu `useEffect` -Hooks können für jede persistente Abfrage erstellt werden, die die React-App verwendet.

+++

<p> </p>
