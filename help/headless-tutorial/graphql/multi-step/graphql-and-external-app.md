---
title: Abfrage AEM Verwendung von GraphQL aus einer externen App - Erste Schritte mit AEM Headless - GraphQL
description: Erste Schritte mit Adobe Experience Manager (AEM) und GraphQL. Sehen Sie sich AEM GraphQL-APIs eine WKND GraphQL React-Beispielanwendung an. Erfahren Sie, wie diese externe App GraphQL-Aufrufe an AEM sendet, um das Erlebnis zu optimieren. Erfahren Sie, wie Sie eine grundlegende Fehlerbehandlung durchführen.
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '1382'
ht-degree: 1%

---

# Abfragen AEM GraphQL aus einer externen App

In diesem Kapitel wird untersucht, wie AEM GraphQL-APIs verwendet werden können, um das Erlebnis in einer externen Anwendung zu fördern.

In diesem Tutorial wird eine einfache React-App verwendet, um Abenteuerinhalte abzufragen und anzuzeigen, die von AEM GraphQL-APIs bereitgestellt werden. Die Verwendung von React ist größtenteils unwichtig, und die verbrauchende externe Anwendung könnte in jedem Rahmen für jede Plattform geschrieben werden.

## Voraussetzungen

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die in den vorherigen Teilen beschriebenen Schritte abgeschlossen wurden.

_IDE-Screenshots in diesem Kapitel stammen von [Visual Studio-Code](https://code.visualstudio.com/)_

Optional können Sie eine Browser-Erweiterung wie [GraphQL-Netzwerkinspektor](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) , um weitere Details zu einer GraphQL-Abfrage anzeigen zu können.

## Ziele

In diesem Kapitel lernen wir Folgendes:

* Starten und Verstehen der Funktionalität der React-Beispielanwendung
* Erfahren Sie, wie Aufrufe von der externen App an AEM GraphQL-Endpunkte gesendet werden.
* Definieren einer GraphQL-Abfrage zum Filtern einer Liste von Abenteuerinhaltsfragmenten nach Aktivität
* Aktualisieren Sie die React-App, um Steuerelemente zum Filtern über GraphQL bereitzustellen, die Liste der Abenteuer nach Aktivität.

## React-App starten

Da sich dieses Kapitel auf die Entwicklung eines Clients zur Verwendung von Inhaltsfragmenten über GraphQL konzentriert, ist das Beispiel [WKND GraphQL React App-Quellcode muss heruntergeladen und eingerichtet werden](../quick-setup/local-sdk.md) auf Ihrem lokalen Computer.

Das Starten der React-App wird im Abschnitt [Schnelleinstellungen](../quick-setup/local-sdk.md) -Kapitel, jedoch können die abgekürzten Anweisungen befolgt werden:

1. Wenn Sie dies noch nicht getan haben, klonen Sie die WKND GraphQL React-Beispielanwendung aus [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Öffnen Sie die WKND GraphQL React-App in Ihrer IDE.

   ![React-App in VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Navigieren Sie in der Befehlszeile zum `react-app` Ordner
1. Starten Sie die WKND GraphQL React-App, indem Sie den folgenden Befehl aus dem Projektstamm ausführen (die `react-app` folder)

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Überprüfen Sie die App unter [http://localhost:3000/](http://localhost:3000/). Die React-Beispielanwendung besteht aus zwei Hauptteilen:

   * Das Starterlebnis dient als Index der WKND Adventures, indem es abgefragt wird. __Abenteuer__ Inhaltsfragmente in AEM mit GraphQL. In diesem Kapitel werden wir diese Ansicht ändern, um die Filterung von Abenteuern nach Aktivität zu unterstützen.

      ![WKND GraphQL-React-App - Starterlebnis](./assets/graphql-and-external-app/react-home-view.png)

   * Das Erlebnis mit Abenteuerdetails verwendet GraphQL, um die spezifische __Abenteuer__ Inhaltsfragment und zeigt weitere Datenpunkte an.

      ![WKND GraphQL-React-App - Detailerlebnis](./assets/graphql-and-external-app/react-details-view.png)

1. Verwenden Sie die Entwicklungs-Tools des Browsers und eine Browsererweiterung wie [GraphQL-Netzwerkinspektor](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) , um die an AEM gesendeten GraphQL-Abfragen und deren JSON-Antworten zu überprüfen. Dieser Ansatz kann verwendet werden, um GraphQL-Anforderungen und -Antworten zu überwachen, um sicherzustellen, dass sie korrekt formuliert sind und ihre Antworten erwartungsgemäß sind.

   ![Raw-Abfrage für AdventureList](assets/graphql-and-external-app/raw-query-chrome-extension.png)

   *GraphQL-Abfrage, die von der React-App an AEM gesendet wird*

   ![GraphQL-JSON-Antwort](assets/graphql-and-external-app/graphql-json-response.png)

   *JSON-Antwort von AEM auf die React-App*

   Die Abfragen und Antworten sollten mit denen in der GraphiQL-IDE übereinstimmen.

   >[!NOTE]
   >
   > Während der Entwicklung ist die React-App so konfiguriert, dass HTTP-Anforderungen über den Webpack Development Server auf AEM übertragen werden. Die React-App sendet Anfragen an  `http://localhost:3000` , die sie an den AEM-Autorendienst weiterleitet, der auf `http://localhost:4502`. Überprüfen Sie die Datei. `src/setupProxy.js` und `env.development` für Details.
   >
   > In Nicht-Entwicklungs-Szenarios wird die React-App direkt konfiguriert, um Anfragen an AEM zu senden.

## GraphQL-Code der App durchsuchen

1. Öffnen Sie in Ihrer IDE die -Datei. `src/api/useGraphQL.js`.

   Dies ist ein [React Effect Hook](https://reactjs.org/docs/hooks-overview.html#effect-hook) , die auf Änderungen am `query`und sendet bei Änderung eine HTTP-POST-Anfrage an den AEM GraphQL-Endpunkt und gibt die JSON-Antwort an die App zurück.

   Jedes Mal, wenn die React-App eine GraphQL-Abfrage durchführen muss, ruft sie diese benutzerdefinierte `useGraphQL(query)` -Erweiterungspunkt, der die an AEM zu sendende GraphQL übergibt.

   Dieser Hook verwendet die einfache `fetch` -Modul, um die HTTP-POST-GraphQL-Anforderung zu erstellen, aber andere Module wie die [Apollo GraphQL-Client](https://www.apollographql.com/docs/react/) kann ähnlich verwendet werden.

1. Öffnen `src/components/Adventures.js` in der IDE, die für die Liste der Abenteuer in der Startansicht verantwortlich ist, und überprüfen Sie den Aufruf der `useGraphQL` Haken.

   Mit diesem Code wird der Standard `query` , `allAdventuresQuery` wie unten in dieser Datei definiert.

   ```javascript
   const [query, setQuery] = useState(allAdventuresQuery);
   ```

   ... und jedes Mal, wenn `query` ändern, wird die `useGraphQL` Der -Erweiterungspunkt wird aufgerufen, wodurch wiederum die GraphQL-Abfrage gegen AEM ausgeführt und die JSON an die `data` -Variable, die dann zum Rendern der Abenteuerliste verwendet wird.

   ```javascript
   const { data, errorMessage } = useGraphQL(query);
   ```

   Die `allAdventuresQuery` ist eine konstante GraphQL-Abfrage, die in der Datei definiert ist und alle Adventure-Inhaltsfragmente ohne Filterung abfragt und nur die Datenpunkte zurückgibt, die die Startansicht rendern müssen.

   ```javascript
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
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
   `;
   ```

1. Öffnen `src/components/AdventureDetail.js`, die React-Komponente, die für die Anzeige des Erlebnisses mit den Erlebnissen der Erlebnisdetails verantwortlich ist. Diese Ansicht fordert ein bestimmtes Inhaltsfragment an, verwendet dessen JCR-Pfad als eindeutige ID und rendert die angegebenen Details.

   Ähnlich wie `Adventures.js`, die benutzerdefinierte `useGraphQL` React Hook wird wiederverwendet, um diese GraphQL-Abfrage gegen AEM durchzuführen.

   Der Pfad des Inhaltsfragments wird aus dem `props` top wird verwendet, um das Inhaltsfragment anzugeben, für das abgefragt werden soll.

   ```javascript
   const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
   ```

   ... und die parametrisierte GraphQL-Abfrage mithilfe der `adventureDetailQuery(..)` Funktion und übergeben an `useGraphQL(query)` , das die GraphQL-Abfrage gegen AEM ausführt und die Ergebnisse an die `data` -Variable.

   ```javascript
   const { data, errorMessage } = useGraphQL(adventureDetailQuery(contentFragmentPath));
   ```

   Die `adventureDetailQuery(..)` -Funktion umbricht einfach eine GraphQL-Filterabfrage, die AEM verwendet `<modelName>ByPath` -Syntax, um ein einzelnes Inhaltsfragment abzufragen, das durch seinen JCR-Pfad identifiziert wird, und alle angegebenen Datenpunkte zurückgibt, die zum Rendern der Details des Abenteuers erforderlich sind.

   ```javascript
   function adventureDetailQuery(_path) {
   return `{
       adventureByPath (_path: "${_path}") {
         item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
         }
       }
   }
   `;
   }
   ```

## Erstellen einer parametrisierten GraphQL-Abfrage

Ändern wir anschließend die React-App, um parametrisierte GraphQL-Abfragen durchzuführen, die die Startansicht durch die Aktivität der Abenteuer einschränken.

1. Öffnen Sie in Ihrer IDE die Datei: `src/components/Adventures.js`. Diese Datei stellt die Abenteuer-Komponente des Starterlebnisses dar, die die Abenteuer-Karten abfragt und anzeigt.
1. Inspect die Funktion `filterQuery(activity)`, das nicht verwendet wird, aber vorbereitet wurde, um eine GraphQL-Abfrage zu formulieren, mit der Abenteuer durch `activity`.

   Beachten Sie, dass Parameter `activity` in die GraphQL-Abfrage als Teil einer `filter` auf `adventureActivity` -Feld, das den Wert dieses Felds an den Wert des Parameters anpasst.

   ```javascript
   function filterQuery(activity) {
       return `
           {
           adventures (filter: {
               adventureActivity: {
               _expressions: [
                   {
                   value: "${activity}"
                   }
                 ]
               }
           }){
               items {
               _path
               adventureTitle
               adventurePrice
               adventureTripLength
               adventurePrimaryImage {
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
       `;
   }
   ```

1. Aktualisieren der React Adventures-Komponente `return` Anweisung zum Hinzufügen von Schaltflächen, die die neuen parametrisierten `filterQuery(activity)` um die Abenteuer zu bieten, die zu listen.

   ```javascript
   function Adventures() {
       ...
       return (
           <div className="adventures">
   
           {/* Add these three new buttons that set the GraphQL query accordingly */}
   
           {/* The first button uses the default `allAdventuresQuery` */}
           <button onClick={() => setQuery(allAdventuresQuery)}>All</button>
   
           {/* The 2nd and 3rd button use the `filterQuery(..)` to filter by activity */}
           <button onClick={() => setQuery(filterQuery('Camping'))}>Camping</button>
           <button onClick={() => setQuery(filterQuery('Surfing'))}>Surfing</button>
   
           <ul className="adventure-items">
           ...
       )
   }
   ```

1. Speichern Sie die Änderungen und laden Sie die React-App im Webbrowser neu. Die drei neuen Schaltflächen werden oben angezeigt. Wenn Sie darauf klicken, werden automatisch AEM für Adventure-Inhaltsfragmente mit der entsprechenden Aktivität neu abgefragt.

   ![Filtern von Abenteuern nach Aktivität](./assets/graphql-and-external-app/filter-by-activity.png)

1. Versuchen Sie, weitere Filterschaltflächen für die Aktivitäten hinzuzufügen: `Rock Climbing`, `Cycling` und `Skiing`

## Verarbeiten von GraphQL-Fehlern

GraphQL ist stark typisiert und kann daher hilfreiche Fehlermeldungen zurückgeben, wenn die Abfrage ungültig ist. Als Nächstes simulieren wir eine falsche Abfrage, um die zurückgegebene Fehlermeldung anzuzeigen.

1. Datei erneut öffnen `src/api/useGraphQL.js`. Inspect Sie das folgende Snippet, um die Fehlerbehandlung anzuzeigen:

   ```javascript
   //useGraphQL.js
   .then(({data, errors}) => {
           //If there are errors in the response set the error message
           if(errors) {
               setErrors(mapErrors(errors));
           }
           //Otherwise if data in the response set the data as the results
           if(data) {
               setData(data);
           }
       })
       .catch((error) => {
           setErrors(error);
       });
   ```

   Die Antwort wird daraufhin überprüft, ob sie eine `errors` -Objekt. Die `errors` -Objekt wird von AEM gesendet, wenn Probleme mit der GraphQL-Abfrage auftreten, z. B. ein nicht definiertes Feld, das auf dem Schema basiert. Wenn keine `errors` -Objekt `data` festgelegt und zurückgegeben.

   Die `window.fetch` enthält `.catch` Anweisung zu *catch* alle häufigen Fehler wie eine ungültige HTTP-Anforderung oder wenn die Verbindung zum Server nicht hergestellt werden kann.

1. Öffnen Sie die Datei `src/components/Adventures.js`.
1. Ändern Sie die `allAdventuresQuery` , um eine ungültige Eigenschaft einzuschließen `adventurePetPolicy`:

   ```javascript
   /**
    * Query for all Adventures
    * adventurePetPolicy has been added beneath items
   */
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           adventurePetPolicy
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
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
   `;
   ```

   Wir wissen, dass `adventurePetPolicy` ist nicht Teil des Abenteuer-Modells, daher sollte dies einen Trigger verursachen.

1. Speichern Sie die Änderungen und kehren Sie zum Browser zurück. Es sollte eine Fehlermeldung wie die folgende angezeigt werden:

   ![Fehler &quot;Ungültige Eigenschaft&quot;](assets/graphql-and-external-app/invalidProperty.png)

   Die GraphQL-API erkennt, dass `adventurePetPolicy` nicht definiert ist im `AdventureModel` und gibt eine entsprechende Fehlermeldung zurück.

1. Inspect die Antwort von AEM mithilfe der Entwicklertools des Browsers, um die `errors` JSON-Objekt:

   ![Fehler JSON-Objekt](assets/graphql-and-external-app/error-json-response.png)

   Die `errors` -Objekt ist ausführlich und enthält Informationen zum Speicherort der fehlerhaften Abfrage und Classification des Fehlers.

1. Zurück zu `Adventures.js` und kehren Sie die Abfrageänderung zurück, um die App in den richtigen Status zu versetzen.

## Herzlichen Glückwunsch!{#congratulations}

Herzlichen Glückwunsch! Sie haben den Code der WKND GraphQL React-Beispielanwendung erfolgreich durchsucht und aktualisiert, um sie mit parametrisierten GraphQL-Abfragen zu filtern und Abenteuer nach Aktivität aufzulisten! Sie haben auch die Möglichkeit erhalten, einige grundlegende Fehlerbehebungen zu untersuchen.

## Nächste Schritte {#next-steps}

Im nächsten Kapitel [Erweiterte Datenmodellierung mit Fragmentverweisen](./fragment-references.md) Sie erfahren, wie Sie mit der Funktion &quot;Fragmentverweis&quot;eine Beziehung zwischen zwei verschiedenen Inhaltsfragmenten erstellen. Außerdem erfahren Sie, wie Sie eine GraphQL-Abfrage ändern, um Felder aus einem referenzierten Modell einzuschließen.
