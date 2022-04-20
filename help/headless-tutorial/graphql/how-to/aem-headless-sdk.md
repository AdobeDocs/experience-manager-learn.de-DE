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
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# AEM Headless-SDK

Das AEM Headless-SDK besteht aus Bibliotheken, die von Clients verwendet werden können, um schnell und einfach mit AEM Headless-APIs über HTTP zu interagieren.

Das AEM Headless-SDK ist für verschiedene Plattformen verfügbar:

+ [AEM Headless SDK für Client-seitige Browser (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM Headless SDK für server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM Headless-SDK für Java™](https://github.com/adobe/aem-headless-client-java)

## GraphQL-Abfragen

Abfrage AEM GraphQL mithilfe von Abfragen (im Gegensatz zu [persistente GraphQL-Abfragen](#persisted-graphql-queries)) ermöglicht es Entwicklern, Abfragen im Code zu definieren und genau anzugeben, welche Inhalte von AEM angefordert werden sollen.

GraphQL-Abfragen sind in der Regel weniger leistungsfähig als persistente Abfragen, da sie mit der HTTP-POST ausgeführt werden, die an den CDN- und AEM Dispatcher-Ebenen weniger zwischenspeicherbar ist.

### Codebeispiele{#graphql-queries-code-examples}

Im Folgenden finden Sie Codebeispiele zum Ausführen einer GraphQL-Abfrage gegen AEM.

+++ JavaScript-Beispiel

Installieren Sie die [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) durch Ausführen der `npm install` aus dem Stammverzeichnis Ihres Node.js-Projekts.

```
$ npm i @adobe/aem-headless-client-js
```

Dieses Codebeispiel zeigt, wie AEM mithilfe der [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm-Modul mit `async/await` Syntax. Das AEM Headless-SDK für JavaScript unterstützt auch [Syntax von Promise](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchQuery(query, queryParams) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runQuery(query, queryParams);
        // The GraphQL data is stored on the response's data key
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Define the GraphQL query in-code
const adventureNamesQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let data = fetchQuery(adventureNamesQuery);
```

+++


+++ React useEffect(..) Beispiel

Installieren Sie die [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) durch Ausführen der `npm install` aus dem Stammverzeichnis Ihres React-Projekts.

```
$ npm i @adobe/aem-headless-client-js
```

In diesem Codebeispiel wird gezeigt, wie die [React useEffect(..) Hook](https://reactjs.org/docs/hooks-effect.html) , um einen asynchronen Aufruf an AEM GraphQL auszuführen.

Verwenden `useEffect` , um den asynchronen GraphQL-Aufruf in React durchzuführen, ist nützlich, da dies:

1. Stellt einen synchronen Wrapper für den asynchronen Aufruf an AEM bereit.
1. Verringert unnötigerweise benötigte AEM.

```javascript
// src/useGraphQL.js

import { useState, useEffect } from 'react';
import AEMHeadless from '@adobe/aem-headless-client-js';

const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/global/endpoint.json'       // The AEM GraphQL endpoint, this can be pulled out to env variables
});

export function useGraphQL(query, queryParams) {
    let [data, setData] = useState(null);
    let [errors, setErrors] = useState(null);
  
    useEffect(() => {
        async function fetchData() {
            try {
                const response = await aemHeadlessClient.runQuery(query, queryParams);
                setData(response.data);
            } catch(error) {
                setErrors(error);
            };
        }
        fetchData();
    }, [query, queryParams]);
  
    return { data, errors }
}
```

Importieren und verwenden Sie die `useGraphQL` -Hook in der React-Komponente AEM.

```javascript
import useGraphQL from 'useGraphQL';

const adventuresQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let { data, errors } = useGraphQL(adventuresQuery);
```

+++

<p> </p>

## Beständige GraphQL-Abfragen

Abfrage AEM GraphQL mit persistenten Abfragen (im Gegensatz zu [reguläre GraphQL-Abfragen](#graphl-queries)) ermöglicht es Entwicklern, eine Abfrage (aber nicht ihre Ergebnisse) in AEM beizubehalten und die Abfrage dann nach Namen auszuführen. Persistente Abfragen ähneln dem Konzept gespeicherter Prozeduren in SQL-Datenbanken.

Persistente Abfragen sind in der Regel leistungsfähiger als normale GraphQL-Abfragen, da persistente Abfragen über HTTP-GET ausgeführt werden, was auf den CDN- und AEM Dispatcher-Ebenen mehr Cache-fähig ist. Persistente Abfragen sind auch effektiv, definieren eine API und entkoppeln die Notwendigkeit, dass der Entwickler die Details der einzelnen Inhaltsfragmentmodelle versteht.

### Codebeispiele{#persisted-graphql-queries-code-examples}

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
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchPersistedQuery(persistedQueryName) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Execute the persisted query using its name
let data = fetchPersistedQuery('wknd/adventureNames');
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

Dieser Code setzt eine persistente Abfrage mit dem Namen voraus `wknd/adventureNames` wurde in der AEM-Autoreninstanz erstellt und in der AEM-Veröffentlichungsinstanz veröffentlicht.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd/endpoint.json'         // The AEM GraphQL endpoint
})

export function fetchPersistedQuery(persistedQueryName) {
  let [data, setData] = useState(null);
  let [errors, setErrors] = useState(null);

  useEffect(async () => {
    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax 
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        setData(response.data);
    }.catch((error) => {
        setErrors(error);
    });

  }, [persistedQueryName]);

  return { data, errors }
}
```

Rufen Sie diesen Code von einer anderen Stelle im React-Code auf.

```javascript
import useGraphL from '...';

let { data, errors } = fetchPersistedQuery('wknd/adventureNames');
```

+++

<p> </p>
