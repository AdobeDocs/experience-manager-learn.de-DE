---
title: Client-Anwendungsintegration - Erweiterte Konzepte von AEM Headless - GraphQL
description: Implementieren Sie persistente Abfragen und integrieren Sie sie in die WKND-App.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 2%

---

# Client-Anwendungsintegration

Im vorherigen Kapitel haben Sie mit dem GraphiQL Explorer persistente Abfragen erstellt und aktualisiert.

Dieses Kapitel führt Sie durch die Schritte, die zur Integration der persistenten Abfragen in die WKND-Client-Anwendung (auch WKND App genannt) mithilfe von HTTP-GET in bestehende Anwendungen erforderlich sind. **React-Komponenten**. Es bietet außerdem eine optionale Herausforderung, Ihre AEM Headless-Lernprozesse anzuwenden und das Know-how zu programmieren, um die WKND-Client-Anwendung zu verbessern.

## Voraussetzungen {#prerequisites}

Dieses Dokument ist Teil eines mehrteiligen Tutorials. Bevor Sie mit diesem Kapitel fortfahren, vergewissern Sie sich bitte, dass die vorherigen Kapitel abgeschlossen sind. Die WKND-Clientanwendung stellt eine Verbindung zu AEM Veröffentlichungsdienst her. Daher ist es wichtig, dass Sie **hat Folgendes in den AEM Veröffentlichungsdienst veröffentlicht**.

* Projektkonfigurationen
* GraphQL-Endpunkte
* Inhaltsfragmentmodelle
* Erstellte Inhaltsfragmente
* GraphQL-persistente Abfragen

Die _IDE-Screenshots in diesem Kapitel stammen von [Visual Studio-Code](https://code.visualstudio.com/)_

### Kapitel 1-4 Lösungspaket (optional) {#solution-package}

Es ist ein Lösungspaket verfügbar, das installiert wird, das die Schritte in der AEM Benutzeroberfläche für die Kapitel 1-4 abschließt. Dieses Paket ist **nicht erforderlich** wenn die vorherigen Kapitel abgeschlossen sind.

1. Download [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. Navigieren Sie AEM zu **Instrumente** > **Implementierung** > **Pakete** für den Zugriff **Package Manager**.
1. Laden Sie das im vorherigen Schritt heruntergeladene Paket (ZIP-Datei) hoch und installieren Sie es.
1. Replizieren Sie das Paket auf den AEM Publish-Dienst

## Ziele {#objectives}

In diesem Tutorial erfahren Sie, wie Sie die Anforderungen für persistente Abfragen mithilfe der [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js).

## Klonen und Ausführen der Beispiel-Client-Anwendung {#clone-client-app}

Um das Tutorial zu beschleunigen, wird eine Starter-React-JS-App bereitgestellt.

1. Klonen Sie die [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) repository:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bearbeiten Sie die `aem-guides-wknd-graphql/advanced-tutorial/.env.development` Datei und Set `REACT_APP_HOST_URI` , um auf Ihren Ziel-AEM-Veröffentlichungsdienst zu verweisen.

   Aktualisieren Sie die Authentifizierungsmethode, wenn Sie eine Verbindung zu einer Autoreninstanz herstellen.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-pxx-eyy.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=
   REACT_APP_BASIC_AUTH_PASS=
   ```

   ![Entwicklungsumgebung für React Apps](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > Die obigen Anweisungen sind, die React-App mit der **AEM-Veröffentlichungsdienst**, jedoch um eine Verbindung mit der **AEM-Autorendienst** ein lokales Entwicklungstoken für Ihre Ziel-AEM as a Cloud Service Umgebung abrufen.
   >
   > Es ist auch möglich, die App mit einem [lokale Autoreninstanz mit dem AEMaaCS-SDK](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) einfache Authentifizierung verwenden.


1. Öffnen Sie ein Terminal und führen Sie die Befehle aus:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Ein neues Browser-Fenster sollte in [http://localhost:3000](http://localhost:3000)


1. Tippen **Campingplatz** > **Yosemite Backpacken** , um die Yosemite Backpacking Abenteuerdetails zu sehen.

   ![Yosemite-Backpacker-Bildschirm](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Öffnen Sie die Entwicklertools des Browsers und überprüfen Sie die `XHR` Anfrage

   ![POST GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   Sie sollten `GET` Anforderungen an den GraphQL-Endpunkt mit dem Projektnamen config (`wknd-shared`), persistenter Abfragename (`adventure-by-slug`), Variablenname (`slug`), Wert (`yosemite-backpacking`) und Sonderzeichen-Kodierungen.

>[!IMPORTANT]
>
>    Wenn Sie sich fragen, warum die GraphQL-API-Anfrage gegen die `http://localhost:3000` und NICHT in Bezug auf die AEM Publish Service-Domäne, überprüfen Sie [Unterhalb des](../multi-step/graphql-and-react-app.md#under-the-hood) aus dem Grundlegenden Tutorial.


## Überprüfen des Codes

Im [Grundlegendes Tutorial - Erstellen einer React-App, die AEM GraphQL-APIs verwendet](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) Schritt hatten wir einige Schlüsseldateien überprüft und erweitert, um praktische Kenntnisse zu erhalten. Bevor Sie die WKND-App verbessern, überprüfen Sie die Schlüsseldateien.

* [Überprüfen des AEMHeadless-Objekts](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [Implementieren, um AEM von GraphQL persistente Abfragen auszuführen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### Überprüfen `Adventures` React-Komponente

Die Hauptansicht der WKND React-App ist die Liste aller Abenteuer und Sie können diese Abenteuer nach Aktivitätstyp filtern, z. B. _Camping, Radfahren_. Diese Ansicht wird von der `Adventures` -Komponente. Im Folgenden finden Sie die wichtigsten Implementierungsdetails:

* Die `src/components/Adventures.js` Aufrufe `useAllAdventures(adventureActivity)` Hook und hier `adventureActivity` -Argument ist der Aktivitätstyp.

* Die `useAllAdventures(adventureActivity)` Der Erweiterungspunkt wird im Abschnitt `src/api/usePersistedQueries.js` -Datei. Basierend auf `adventureActivity` -Wert bestimmt, welche persistente Abfrage aufgerufen werden soll. Wenn kein Nullwert angegeben wird, wird `wknd-shared/adventures-by-activity`, sonst erhalten alle verfügbaren Abenteuer `wknd-shared/adventures-all`.

* Der Haken verwendet das Haupt `fetchPersistedQuery(..)` -Funktion, die die Ausführung der Abfrage an `AEMHeadless` via `aemHeadlessClient.js`.

* Der Erweiterungspunkt gibt auch nur die relevanten Daten aus der AEM GraphQL-Antwort zurück unter `response.data?.adventureList?.items`, wodurch die `Adventures` React-View-Komponenten müssen unabhängig von den übergeordneten JSON-Strukturen sein.

* Bei erfolgreicher Ausführung der Abfrage wird die `AdventureListItem(..)` Render-Funktion von `Adventures.js` fügt HTML-Element hinzu, um die _Bild, Reisedauer, Preis und Titel_ Informationen.

### Überprüfen `AdventureDetail` React-Komponente

Die `AdventureDetail` Die React-Komponente rendert die Details des Abenteuers. Im Folgenden finden Sie die wichtigsten Implementierungsdetails:

* Die `src/components/AdventureDetail.js` Aufrufe `useAdventureBySlug(slug)` Hook und hier `slug` -Argument ist Abfrageparameter.

* Wie oben, wird die `useAdventureBySlug(slug)` Der Erweiterungspunkt wird im Abschnitt `src/api/usePersistedQueries.js` -Datei. Sie ruft `wknd-shared/adventure-by-slug` persistente Abfrage durch Delegieren an `AEMHeadless` via `aemHeadlessClient.js`.

* Bei erfolgreicher Ausführung der Abfrage wird die `AdventureDetailRender(..)` Render-Funktion von `AdventureDetail.js` fügt ein HTML-Element hinzu, um die Details des Abenteuers anzuzeigen.


## Verbessern des Codes

### Verwendung `adventure-details-by-slug` persistente Abfrage

Im vorherigen Kapitel haben wir die `adventure-details-by-slug` persistente Abfrage enthält, werden zusätzliche Abenteuerinformationen bereitgestellt, z. B. _location, lehrerteam und administrator_. Lasst uns ersetzen `adventure-by-slug` mit `adventure-details-by-slug` persistente Abfrage zum Rendern dieser zusätzlichen Informationen.

1. Öffnen Sie `src/api/usePersistedQueries.js`.

1. Suchen Sie die Funktion `useAdventureBySlug()` und aktualisieren Sie die Abfrage als

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### Zusätzliche Informationen anzeigen

1. Um weitere Abenteuerinformationen anzuzeigen, öffnen Sie `src/components/AdventureDetail.js`

1. Suchen Sie die Funktion `AdventureDetailRender(..)` und aktualisieren Sie die Rückgabefunktion als

   ```javascript
   ...
   
   return (<>
       <h1 className="adventure-detail-title">{title}</h1>
       <div className="adventure-detail-info">
   
           <LocationInfo {...location} />
   
           ...
   
           <Location {...location} />
   
           <Administrator {...administrator} />
   
           <InstructorTeam {...instructorTeam} />
   
       </div>
   </>); 
   
   ...
   ```

1. Definieren Sie auch die entsprechenden Renderfunktionen:

   **LocationInfo**

   ```javascript
   function LocationInfo({name}) {
   
       if (!name) {
           return null;
       }
   
       return (
           <>
               <div className="adventure-detail-info-label">Location</div>
               <div className="adventure-detail-info-description">{name}</div>
           </>
       );
   
   }
   ```

   **Speicherort**

   ```javascript
   function Location({ contactInfo }) {
   
       if (!contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-location'>
                   <h2>Where we meet</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Phone:{contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email:{contactInfo.email}</div>
               </div>
           </>);
   }
   ```

   **InstructorTeam**

   ```javascript
   function InstructorTeam({ _metadata }) {
   
       if (!_metadata) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-team'>
                   <h2>Instruction Team</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Team Name: {_metadata.stringMetadata[0].value}</div>
               </div>
           </>);
   }
   ```

   **Administrator**

   ```javascript
   function Administrator({ fullName, contactInfo }) {
   
       if (!fullName || !contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-administrator'>
                   <h2>Administrator</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Name: {fullName}</div>
                   <div className="adventure-detail-addtional-info">Phone: {contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email: {contactInfo.email}</div>
               </div>
           </>);
   }
   ```

### Neue Stile definieren

1. Öffnen `src/components/AdventureDetail.scss` und fügen Sie die folgenden Klassendefinitionen hinzu

   ```CSS
   .adventure-detail-administrator,
   .adventure-detail-team,
   .adventure-detail-location {
   margin-top: 1em;
   width: 100%;
   float: right;
   }
   
   .adventure-detail-addtional-info {
   padding: 10px 0px 5px 0px;
   text-transform: uppercase;
   }
   ```

>[!TIP]
>
>Die aktualisierten Dateien finden Sie unter **AEM Guides WKND - GraphQL** Projekt, siehe [Erweitertes Tutorial](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) Abschnitt.


Nach Abschluss der oben genannten Verbesserungen sieht die WKND-App wie folgt aus und die Entwicklertools des Browsers zeigen `adventure-details-by-slug` persistenter Abfrageaufruf.

![Verbessertes WKND-APP](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Verbesserungsproblem (optional)

Die Hauptansicht der WKND React-App ermöglicht es Ihnen, diese Abenteuer nach Aktivitätstyp wie _Camping, Radfahren_. Das WKND-Geschäftsteam möchte jedoch eine zusätzliche _Standort_ Filterfunktion. Die Anforderungen sind

* Fügen Sie in der Hauptansicht der WKND-App oben links oder rechts hinzu _Standort_ Filtersymbol.
* Klicken _Standort_ über das Filtersymbol sollte die Liste der Standorte angezeigt werden.
* Wenn Sie in der Liste auf eine gewünschte Positionsoption klicken, sollte nur die passenden Abenteuer angezeigt werden.
* Wenn nur ein Abenteuer übereinstimmt, wird die Ansicht Abenteuer Details angezeigt.

## Herzlichen Glückwunsch

Herzlichen Glückwunsch! Sie haben die Integration abgeschlossen und die persistenten Abfragen in die WKND-Beispielanwendung implementiert.
