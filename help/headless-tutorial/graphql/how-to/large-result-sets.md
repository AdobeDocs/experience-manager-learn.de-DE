---
title: Arbeiten mit großen Ergebnismengen in AEM Headless
description: Erfahren Sie, wie Sie mit großen Ergebnismengen in AEM Headless arbeiten.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
exl-id: 304b4d80-27bd-4336-b2ff-4b613a30f712
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: ht
source-wordcount: '841'
ht-degree: 100%

---

# Große Ergebnismengen in AEM Headless

AEM Headless-GraphQL-Abfragen können umfangreiche Ergebnisse zurückgeben. In diesem Artikel wird beschrieben, wie Sie mit umfangreichen Ergebnissen in AEM Headless arbeiten, um die beste Leistung für Ihre Anwendung sicherzustellen.

AEM Headless unterstützt einen [Offset-/Limit-Ansatz](#list-query) und Abfragen mit [Cursor-basierter Paginierung](#paginated-query) für kleinere Teilmengen eines größeren Ergebnissatzes. Es können mehrere Anfragen gestellt werden, um so viele Ergebnisse wie erforderlich zu erfassen.

In den folgenden Beispielen werden kleine Teilmengen von Ergebnissen (vier Datensätze pro Anfrage) verwendet, um die Techniken zu zeigen. In einer echten Anwendung würden Sie eine größere Anzahl von Datensätzen pro Anfrage verwenden, um die Leistung zu verbessern. 50 Datensätze pro Anfrage sind eine gute Grundlage.

## Inhaltsfragmentmodell

Paginierung und Sortierung können für jedes Inhaltsfragmentmodell verwendet werden.

## GraphQL-persistierte Abfragen

Beim Arbeiten mit großen Datensätzen können Offset/Limit und die Cursor-basierte Paginierung verwendet werden, um eine bestimmte Teilmenge von Daten abzurufen. Es gibt jedoch gewisse Unterschiede zwischen den beiden Techniken, weswegen in bestimmten Situationen die eine geeigneter ist als die andere.

### Offset/Limit

Listenabfragen mithilfe von `limit` und `offset` stellen einen einfachen Ansatz zur Festlegung des Ausgangspunkts (`offset`) und der Anzahl der abzurufenden Datensätze (`limit`) dar. Dieser Ansatz ermöglicht die Auswahl einer Teilmenge von Ergebnissen an einer beliebigen Stelle innerhalb des vollständigen Ergebnissatzes, z. B. durch Springen zu einer bestimmten Ergebnisseite. Er lässt sich zwar einfach implementieren, kann aber bei umfangreichen Ergebnissen langsam und ineffizient sein, da zum Abrufen vieler Datensätze das Durchsuchen aller vorherigen Datensätze erforderlich ist. Dieser Ansatz kann auch zu Leistungsproblemen führen, wenn der Offset-Wert hoch ist, da unter Umständen viele Ergebnisse abgerufen und dann wieder verworfen werden müssen.

#### GraphQL-Abfrage

```graphql
# Retrieves a list of Adventures sorted price descending, and title ascending if there is the prices are the same.
query adventuresByOffetAndLimit($offset:Int!, $limit:Int) {
    adventureList(offset: $offset, limit: $limit, sort: "price DESC, title ASC", ) {
      items {
        _path
        title
        price
      }
    }
  }
```

##### Abfragevariablen

```json
{
  "offset": 1,
  "limit": 4
}
```

#### GraphQL-Antwort

Die resultierende JSON-Antwort enthält das zweit-, dritt-, viert- und fünftteuerste Abenteuer. Die ersten beiden Abenteuer in den Ergebnissen haben den gleichen Preis (`4500`, sodass die [Listenabfrage](#list-queries) Abenteuer mit dem gleichen Preis nach Titel in aufsteigender Reihenfolge sortiert).

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
          "title": "Cycling Tuscany",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
          "title": "West Coast Cycling",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica",
          "title": "Surf Camp in Costa Rica",
          "price": 3400
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah",
          "title": "Cycling Southern Utah",
          "price": 3000
        }
      ]
    }
  }
}
```

### Paginierte Abfrage

Die Cursor-basierte Paginierung, die in paginierten Abfragen verfügbar ist, beinhaltet die Verwendung eines Cursors (ein Verweis auf einen bestimmten Datensatz), um den nächsten Ergebnissatz abzurufen. Dieser Ansatz ist effizienter, da es nicht notwendig ist, alle vorherigen Datensätze zu durchsuchen, um die erforderliche Teilmenge von Daten abzurufen. Paginierte Abfragen eignen sich hervorragend für die Iteration großer Ergebnismengen von Anfang bis zu einem Punkt in der Mitte oder bis zum Ende. Listenabfragen mithilfe von `limit` und `offset` stellen einen einfachen Ansatz zur Festlegung des Ausgangspunkts (`offset`) und der Anzahl der abzurufenden Datensätze (`limit`) dar. Dieser Ansatz ermöglicht die Auswahl einer Teilmenge von Ergebnissen an einer beliebigen Stelle innerhalb des vollständigen Ergebnissatzes, z. B. durch Springen zu einer bestimmten Ergebnisseite. Er lässt sich zwar einfach implementieren, kann aber bei umfangreichen Ergebnissen langsam und ineffizient sein, da zum Abrufen vieler Datensätze das Durchsuchen aller vorherigen Datensätze erforderlich ist. Dieser Ansatz kann auch zu Leistungsproblemen führen, wenn der Offset-Wert hoch ist, da unter Umständen viele Ergebnisse abgerufen und dann wieder verworfen werden müssen.

#### GraphQL-Abfrage

```graphql
# Retrieves the most expensive Adventures (sorted by title ascending if there is the prices are the same)
query adventuresByPaginated($first:Int, $after:String) {
 adventurePaginated(first: $first, after: $after, sort: "price DESC, title ASC") {
       edges {
          cursor
          node {
            _path
            title
            price
          }
        }
        pageInfo {
          endCursor
          hasNextPage
        }
    }
  }
```

##### Abfragevariablen

```json
{
  "first": 3
}
```

#### GraphQL-Antwort

Die resultierende JSON-Antwort enthält das zweit-, dritt-, viert- und fünftteuerste Abenteuer. Die ersten beiden Abenteuer in den Ergebnissen haben den gleichen Preis (`4500`, sodass die [Listenabfrage](#list-queries) Abenteuer mit dem gleichen Preis nach Titel in aufsteigender Reihenfolge sortiert).

```json
{
  "data": {
    "adventurePaginated": {
      "edges": [
        {
          "cursor": "NTAwMC4...Dg0ZTUwN2FkOA==",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
            "title": "Bali Surf Camp",
            "price": 5000
          }
        },
        {
          "cursor": "SFNDUwMC4wC...gyNWUyMWQ5M2Q=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
            "title": "Cycling Tuscany",
            "price": 4500
          }
        },
        {
          "cursor": "AVUwMC4w...0ZTYzMjkwMzE5Njc=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
            "title": "West Coast Cycling",
            "price": 4500
          }
        }
      ],
      "pageInfo": {
        "endCursor": "NDUwMC4w...kwMzE5Njc=",
        "hasNextPage": true
      }
    }
  }
}
```

#### Nächster Satz paginierter Ergebnisse

Der nächste Ergebnissatz kann mit dem `after`-Parameter und dem `endCursor`-Wert aus der vorherigen Abfrage abgerufen werden. Wenn keine weiteren Ergebnisse abgerufen werden müssen, ist `hasNextPage` gleich `false`.

##### Abfragevariablen

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## React-Beispiele

Im Folgenden finden Sie React-Beispiele, die zeigen, wie Sie die Ansätze [Offset und Limit](#offset-and-limit) sowie [Cursor-basierte Paginierung](#cursor-based-pagination) verwenden. In der Regel ist die Anzahl der Ergebnisse pro Anfrage größer, für die Zwecke dieser Beispiele ist sie jedoch auf 5 begrenzt.

### Beispiel für Offset und Limit

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

Mithilfe von Offset und Limit können Teilmengen von Ergebnissen einfach abgerufen und angezeigt werden.

#### useEffect-Hook

Der `useEffect`-Hook startet eine persistierte Abfrage (`adventures-by-offset-and-limit`), die eine Liste von Abenteuern abruft. Die Abfrage verwendet die Parameter `offset` und `limit`, um den Ausgangspunkt und die Anzahl der abzurufenden Ergebnisse anzugeben. Die `useEffect`-Hook wird gestartet, wenn sich der `page`-Wert ändert.


```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function useOffsetLimitAdventures(page, limit) {
    const [adventures, setAdventures] = useState([]);
    const [hasMore, setHasMore] = useState(true);

    useEffect(() => {
      async function fetchData() {
        const queryParameters = {
          offset: page * limit, // Calculate the offset based on the current page and the limit
          limit: limit + 1,     // Add 1 to the limit to determine if there are more adventures to fetch
        };

        // Invoke the persisted query with the offset and limit parameters
        const response = await aemHeadlessClient.runPersistedQuery(
          "wknd-shared/adventures-by-offset-and-limit",
          queryParameters
        );        
        const data = response?.data;

        if (data?.adventureList?.items?.length > 0) {
          // Collect the adventures - slice off the last item since the last item is used to determine if there are more adventures to fetch
          setAdventures([...data.adventureList.items].slice(0, limit));
          // Determine if there are more adventures to fetch
          setHasMore(data.adventureList.items.length > limit);
        } else {
          setHasMore(false);
        }
      }
      fetchData();
    }, [page]);

    return { adventures, hasMore };
}
```

#### Komponente

Die Komponente verwendet den `useOffsetLimitAdventures`-Hook zum Abrufen einer Liste von Abenteuern. Der `page`-Wert wird erhöht oder verringert, um den nächsten bzw. vorherigen Ergebnissatz abzurufen. Der `hasMore`-Wert wird verwendet, um zu bestimmen, ob die Schaltfläche „Nächste Seite“ aktiviert werden soll.

```javascript
import { useState } from "react";
import { useOffsetLimitAdventures } from "./api/persistedQueries";

export default function OffsetLimitAdventures() {
  const LIMIT = 5;
  const [page, setPage] = useState(0);

  let { adventures, hasMore } = useOffsetLimitAdventures(page, LIMIT);

  return (
    <section className="offsetLimit">
      <h2>Offset/limit query</h2>
      <p>Collect sub-sets of adventures using offset and limit.</p>

      <h4>Page: {page + 1}</h4>
      <p>
        Query variables:
        <em>
          <code>
            &#123; offset: {page * LIMIT}, limit: {LIMIT} &#125;
          </code>
        </em>
      </p>

      <hr />

      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure._path}>
              {adventure.title} <em>(${adventure.price})</em>
            </li>
          );
        })}
      </ul>

      <hr />

      <ul className="buttons">
        <li>
          <button disabled={page === 0} onClick={() => setPage(page - 1)}>
            Previous
          </button>
        </li>
        <li>
          <button disabled={!hasMore} onClick={() => setPage(page + 1)}>
            Next
          </button>
        </li>
      </ul>
    </section>
  );
}
```

### Beispiel für eine paginierte Abfrage

![Beispiel für eine paginierte Abfrage](./assets/large-results/paginated-example.png)

_Jeder rote Rahmen kennzeichnet eine separate paginierte HTTP-GraphQL-Abfrage._

Mithilfe der Cursor-basierten Paginierung können große Ergebnismengen einfach abgerufen und angezeigt werden, indem die Ergebnisse schrittweise erfasst und mit den vorhandenen Ergebnissen verkettet werden.


#### UseEffect-Hook

Der `useEffect`-Hook startet eine persistierte Abfrage (`adventures-by-paginated`), die eine Liste von Abenteuern abruft. Die Abfrage verwendet die Parameter `first` und `after`, um die Anzahl der Ergebnisse, die abgerufen werden sollen, und den Startpunkt für den Cursor anzugeben. `fetchData` wird in einer Endlosschleife ausgeführt und erfasst den nächsten Satz paginierter Ergebnisse, bis keine Ergebnisse mehr abgerufen werden können.

```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function usePaginatedAdventures() {
    const LIMIT = 5;
    const [adventures, setAdventures] = useState([]);
    const [queryCount, setQueryCount] = useState(0);

    useEffect(() => {
      async function fetchData() {
        let paginatedAdventures = [];
        let paginatedCount = 0;
        let hasMore = false;
        let after = null;
        
        do {
          const response = await aemHeadlessClient.runPersistedQuery(
            "wknd-shared/adventures-by-paginated",
            {
                first: LIMIT,
                after: after
            }
          );
          // The GraphQL data is stored on the response's data field
          const data = response?.data;

          paginatedCount = paginatedCount + 1;

          if (data?.adventurePaginated?.edges?.length > 0) {
            // Add the next set page of adventures to full list of adventures
            paginatedAdventures = [...paginatedAdventures, ...data.adventurePaginated.edges];
          }

          // If there are more adventures, set the state to fetch them
          hasMore = data.adventurePaginated?.pageInfo?.hasNextPage;
          after = data.adventurePaginated.pageInfo.endCursor;

        } while (hasMore);

        setQueryCount(paginatedCount);
        setAdventures(paginatedAdventures);
      }

      fetchData();
    }, []);

    return { adventures, queryCount };
}
```

#### Komponente

Die Komponente verwendet den `usePaginatedAdventures`-Hook zum Abrufen einer Liste von Abenteuern. Der `queryCount`-Wert wird verwendet, um die Anzahl der HTTP-Anfragen anzuzeigen, die zum Abrufen der Liste von Abenteuern gesendet wurden.

```javascript
import { useState } from "react";
import { usePaginatedAdventures } from "./api/persistedQueries";
...
export default function PaginatedAdventures() {
  let { adventures, queryCount } = usePaginatedAdventures();

  return (
    <section className="paginated">
      <h2>Paginated query</h2>
      <p>Collect all adventures using {queryCount} cursor-paginated HTTP GraphQL requests</p>

      <hr/>
      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure.node._path}>
              {adventure.node.title} <em>(${adventure.node.price})</em>
            </li>
          );
        })}
      </ul>
      <hr/>
    </section>
  );
}
```
