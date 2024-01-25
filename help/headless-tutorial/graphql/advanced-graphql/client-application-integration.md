---
title: Client-Anwendungsintegration – Erweiterte AEM Headless-Konzepte – GraphQL
description: Implementieren Sie persistierte Abfragen und integrieren Sie sie in die WKND-App.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
duration: 301
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 100%

---

# Client-Anwendungsintegration

Im vorherigen Kapitel haben Sie mit dem GraphiQL-Explorer persistierte Abfragen erstellt und aktualisiert.

Dieses Kapitel führt Sie durch die Schritte, die zur Integration der persistierten Abfragen in die WKND-Client-Anwendung (auch als WKND-App bezeichnet) mithilfe von HTTP-GET-Anfragen innerhalb vorhandener **React-Komponenten** erforderlich sind. Es umfasst außerdem eine optionale Herausforderung: Ihr erworbenes AEM Headless-Wissen und Programmier-Know-how zur Erweiterung der WKND-Client-Anwendung anzuwenden.

## Voraussetzungen {#prerequisites}

Dieses Dokument ist Teil eines mehrteiligen Tutorials. Bevor Sie mit diesem Kapitel fortfahren, vergewissern Sie sich, dass Sie die vorherigen Kapitel abgeschlossen haben. Die WKND-Client-Anwendung stellt eine Verbindung zum AEM-Veröffentlichungs-Service her. Daher ist es wichtig, dass Sie **Folgendes im AEM-Veröffentlichungs-Service veröffentlicht haben**.

* Projektkonfigurationen
* GraphQL-Endpunkte
* Inhaltsfragmentmodelle
* Erstellte Inhaltsfragmente
* GraphQL – Persistierte Abfragen

Die _IDE-Screenshots in diesem Kapitel stammen von [Visual Studio Code](https://code.visualstudio.com/)_.

### Kapitel 1–4 Lösungspaket (optional) {#solution-package}

Es ist ein Lösungspaket zur Installation verfügbar, das die Schritte in der AEM-Benutzeroberfläche für die Kapitel 1–4 abschließt. Dieses Paket ist **nicht erforderlich**, wenn die vorherigen Kapitel abgeschlossen wurden.

1. Laden Sie die Datei [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) herunter.
1. Navigieren Sie in AEM zu **Tools** > **Bereitstellung** > **Pakete**, um auf **Package Manager** zuzugreifen.
1. Laden Sie das im vorherigen Schritt heruntergeladene Paket (die ZIP-Datei) hoch und installieren Sie es.
1. Replizieren Sie das Paket für den AEM-Veröffentlichungs-Service.

## Ziele {#objectives}

In diesem Tutorial erfahren Sie, wie Sie Anfragen für persistierte Abfragen mit dem [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js) in die beispielhafte WKND GraphQL React-App integrieren.

## Klonen und Ausführen der beispielhaften Client-Anwendung {#clone-client-app}

Um während des Tutorials Zeit zu sparen, wird eine React-JS-App als Start bereitgestellt.

1. Klonen Sie das Repository [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bearbeiten Sie die Datei `aem-guides-wknd-graphql/advanced-tutorial/.env.development` und legen Sie `REACT_APP_HOST_URI` so fest, dass auf den AEM Publish-Ziel-Service verwiesen wird.

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

   ![React-App-Entwicklungsumgebung](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > Die obigen Anweisungen sehen vor, die React-App mit dem **AEM-Veröffentlichungs-Service** zu verbinden. Um allerdings eine Verbindung zum **AEM-Autoren-Service** herzustellen, ist ein lokales Entwicklungs-Token für Ihre AEM as a Cloud Service-Zielumgebung erforderlich.
   >
   > Es ist ebenfalls möglich, die App mit einer [lokalen Autoreninstanz über das AEMaaCS-SDK](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) mittels einfacher Authentifizierung zu verbinden.


1. Öffnen Sie ein Terminal und führen Sie die folgenden Befehle aus:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Ein neues Browser-Fenster sollte unter [http://localhost:3000](http://localhost:3000) geladen werden.


1. Tippen Sie auf **Camping** > **Yosemite Backpacking**, um Details zum Yosemite Backpacking-Adventure anzuzeigen.

   ![Yosemite Backpacking-Bildschirm](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Öffnen Sie die Entwickler-Tools des Browsers und überprüfen Sie die `XHR`-Anfrage

   ![POST GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   Es sollten `GET`-Anfragen an den GraphQL-Endpunkt mit dem Konfigurationsnamen des Projekts (`wknd-shared`), dem Namen der persistierten Abfrage (`adventure-by-slug`), dem Variablennamen (`slug`), dem Wert (`yosemite-backpacking`) und den Sonderzeichen-Codierungen zu sehen sein.

>[!IMPORTANT]
>
>    Wenn Sie sich fragen, warum die GraphQL-API-Anfrage an `http://localhost:3000` und NICHT an die Domain des AEM-Veröffentlichungs-Service gerichtet ist, finden Sie im grundlegenden Tutorial unter [Unter der Haube](../multi-step/graphql-and-react-app.md#under-the-hood) weitere Informationen dazu.


## Überprüfen des Codes

Im [Schritt zum Erstellen einer React-App mit Nutzung von AEM GraphQL-APIs des grundlegenden Tutorials](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html?lang=de#review-the-aemheadless-object) hatten wir einige Schlüsseldateien überprüft und erweitert, um praktische Kenntnisse zu vermitteln. Bevor Sie die WKND-App verbessern, sehen Sie sich diese Schlüsseldateien an.

* [Überprüfen des AEMHeadless-Objekts](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html?lang=de#review-the-aemheadless-object)

* [Implementieren zum Ausführen von AEM GraphQL-persistierten Abfragen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html?lang=de#implement-to-run-aem-graphql-persisted-queries)

### Überprüfen der React-Komponente `Adventures`

Die Hauptansicht der WKND React-App entspricht der Liste aller Adventures. Sie können diese Adventures nach Aktivitätstyp filtern, z. B. _Camping, Cycling_. Diese Ansicht wird von der `Adventures`-Komponente gerendert. Im Folgenden finden Sie die wichtigsten Implementierungsdetails:

* `src/components/Adventures.js` ruft den Hook `useAllAdventures(adventureActivity)` auf, wobei hier das Argument `adventureActivity` für den Aktivitätstyp steht.

* Der Hook `useAllAdventures(adventureActivity)` ist in der Datei `src/api/usePersistedQueries.js` definiert. Basierend auf dem Wert von `adventureActivity`wird bestimmt, welche persistierte Abfrage aufgerufen werden soll. Sofern der Wert nicht null ist, wird `wknd-shared/adventures-by-activity` aufgerufen, andernfalls alle verfügbaren Adventures (`wknd-shared/adventures-all`).

* Der Hook verwendet als Hauptfunktion `fetchPersistedQuery(..)`, die die Ausführung der Abfrage über `aemHeadlessClient.js` an `AEMHeadless` delegiert.

* Der Hook gibt auch nur die relevanten Daten aus der AEM GraphQL-Antwort unter `response.data?.adventureList?.items` zurück, sodass die React-Ansichtskomponente `Adventures` unabhängig von den übergeordneten JSON-Strukturen sein kann.

* Bei erfolgreicher Ausführung der Abfrage fügt die Render-Funktion `AdventureListItem(..)` von `Adventures.js` ein HTML-Element hinzu, um _Bild-, Reisedauer-, Preis- und Titelinformationen_ anzuzeigen.

### Überprüfen der React-Komponente `AdventureDetail`

Die React-Komponente `AdventureDetail` rendert die Details des Adventures. Im Folgenden finden Sie die wichtigsten Implementierungsdetails:

* `src/components/AdventureDetail.js` ruft den Hook `useAdventureBySlug(slug)` auf, wobei hier das Argument `slug` für den Abfrageparameter steht.

* Wie oben ist der Hook `useAdventureBySlug(slug)` in der Datei `src/api/usePersistedQueries.js` definiert. Die persistierte Abfrage `wknd-shared/adventure-by-slug` wird durch Delegieren über `aemHeadlessClient.js` an `AEMHeadless` aufgerufen.

* Bei erfolgreicher Ausführung der Abfrage fügt die Render-Funktion `AdventureDetailRender(..)` von `AdventureDetail.js` ein HTML-Element hinzu, um Details zum Adventure anzuzeigen.


## Erweitern des Codes

### Verwenden der persistierten Abfrage `adventure-details-by-slug`

Im vorherigen Kapitel haben wir die persistierte Abfrage `adventure-details-by-slug` erstellt. Diese liefert zusätzliche Informationen zum Adventure, z. B. zum _Ort, Lehrpersonal und Administrator-Team_. Ersetzen Sie `adventure-by-slug` durch die persistierte Abfrage `adventure-details-by-slug`, um diese zusätzlichen Informationen zu rendern.

1. Öffnen Sie `src/api/usePersistedQueries.js`.

1. Suchen Sie die Funktion `useAdventureBySlug()` und aktualisieren Sie die Abfrage wie folgt:

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### Anzeigen zusätzlicher Informationen

1. Um weitere Adventure-Informationen anzuzeigen, öffnen Sie `src/components/AdventureDetail.js`.

1. Suchen Sie die Funktion `AdventureDetailRender(..)` und aktualisieren Sie die Rückgabefunktion wie folgt:

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

1. Definieren Sie auch die entsprechenden Render-Funktionen:

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

### Definieren neuer Stile

1. Öffnen Sie `src/components/AdventureDetail.scss` und fügen Sie die folgenden Klassendefinitionen hinzu:

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
>Die aktualisierten Dateien finden Sie unter dem Projekt **AEM Guides WKND – GraphQL** (siehe Abschnitt [Erweitertes Tutorial](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial)).


Nach Abschluss der oben genannten Verbesserungen sieht die WKND-App wie folgt aus und die Entwickler-Tools des Browsers zeigen den Aufruf der persisenten Abfrage `adventure-details-by-slug` an.

![Erweiterte WKND-App](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Erweiterungsproblem (optional)

Die Hauptansicht der WKND React-App ermöglicht es Ihnen, diese Adventures nach Aktivitätstyp wie _Camping, Cycling_ zu filtern. Das WKND-Business-Team wünscht sich jedoch eine zusätzliche _standortbasierte_ Filterfunktion. Die Anforderungen lauten:

* In der Hauptansicht der WKND-App ist oben links oder rechts ein Filterssymbol für _Standort_ hinzuzufügen.
* Durch Klicken auf das Filtersymbol für _Standorte_ sollte die Liste der Standorte angezeigt werden.
* Durch Klicken auf eine gewünschte Standortoption in der Liste sollten nur die dazu passenden Adventures angezeigt werden.
* Wenn nur ein Adventure passend ist, wird die Ansicht mit den Adventure-Details angezeigt.

## Herzlichen Glückwunsch

Herzlichen Glückwunsch! Sie haben nun die Integration abgeschlossen und die persistierten Abfragen in die WKND-Beispielanwendung implementiert.
