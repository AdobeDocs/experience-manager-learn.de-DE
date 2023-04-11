---
title: Erstellen Sie eine React-App, die mit der GraphQL-API Abfragen AEM - Erste Schritte mit AEM Headless - GraphQL
description: Erste Schritte mit Adobe Experience Manager (AEM) und GraphQL. Erstellen Sie eine React-App, die Inhalte/Daten aus AEM GraphQL-API abruft. Außerdem sehen Sie, wie AEM Headless JS SDK verwendet wird.
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1182'
ht-degree: 2%

---


# Erstellen einer React-App, die AEM GraphQL-APIs verwendet

In diesem Kapitel erfahren Sie, wie AEM GraphQL-APIs das Erlebnis in einer externen Anwendung fördern können.

Eine einfache React-App wird zum Abfragen und Anzeigen verwendet **Team** und **Person** Inhalte, die von AEM GraphQL-APIs bereitgestellt werden. Die Verwendung von React ist größtenteils unwichtig, und die verbrauchende externe Anwendung könnte in jedem Rahmen für jede Plattform geschrieben werden.

## Voraussetzungen

Es wird davon ausgegangen, dass die in den vorherigen Teilen dieses mehrteiligen Tutorials beschriebenen Schritte abgeschlossen wurden, oder [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip) wird auf Ihrem AEM as a Cloud Service Autoren- und Veröffentlichungsdienst installiert.

_IDE-Screenshots in diesem Kapitel stammen von [Visual Studio-Code](https://code.visualstudio.com/)_

Die folgende Software muss installiert sein:

- [Node.js v18](https://nodejs.org/)
- [Visual Studio-Code](https://code.visualstudio.com/)

## Ziele

Erfahren Sie mehr über:

- Herunterladen und Starten der React-Beispielanwendung
- Abfragen AEM GraphQL-Endpunkte mithilfe der [AEM Headless JS SDK](https://github.com/adobe/aem-headless-client-js)
- AEM für eine Liste von Teams und deren referenzierte Mitglieder abfragen
- Abfrage AEM Details eines Teammitglieds

## Beispiel-React-App abrufen

In diesem Kapitel wird eine ausgesetzte React-Beispielanwendung implementiert, die den Code enthält, der für die Interaktion mit AEM GraphQL-API erforderlich ist, und Teams- und Personendaten anzeigt, die von ihnen erfasst wurden.

Der Beispiel-Quell-Code der React-App ist auf Github.com unter <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

So rufen Sie die React-App ab:

1. Klonen Sie die WKND GraphQL React-App-Beispiel aus [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navigieren Sie zu `basic-tutorial` und öffnen Sie sie in Ihrer IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![React-App in VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Aktualisieren `.env.development` , um eine Verbindung zum as a Cloud Service Veröffentlichungsdienst AEM.

   - Satz `REACT_APP_HOST_URI`Der Wert von ist die Veröffentlichungs-URL Ihres AEM as a Cloud Service (z. B. `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) und `REACT_APP_AUTH_METHOD`Wert von `none`
   >[!NOTE]
   >
   > Vergewissern Sie sich, dass Sie die Projektkonfiguration, Inhaltsfragmentmodelle, erstellten Inhaltsfragmente, GraphQL-Endpunkte und persistente Abfragen aus vorherigen Schritten veröffentlicht haben.
   >
   > Wenn Sie die oben genannten Schritte für das lokale AEM Author-SDK ausgeführt haben, können Sie auf `http://localhost:4502` und `REACT_APP_AUTH_METHOD`Wert von `basic`.


1. Wechseln Sie in der Befehlszeile zum `aem-guides-wknd-graphql/basic-tutorial` Ordner

1. React-App starten

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. Die React-App startet im Entwicklungsmodus auf [http://localhost:3000/](http://localhost:3000/). Änderungen, die während des Tutorials an der React-App vorgenommen wurden, werden sofort übernommen.

![Teilweise implementierte React-App](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Diese React-App ist teilweise implementiert. Führen Sie die Schritte in diesem Tutorial aus, um die Implementierung abzuschließen. Für die JavaScript-Dateien, für die Implementierungsarbeiten erforderlich sind, gibt es den folgenden Kommentar: Stellen Sie sicher, dass Sie den Code in diesen Dateien mit dem in diesem Tutorial angegebenen Code hinzufügen/aktualisieren.
>
>
> //*******************************************
>
>  // TODO : Implementieren Sie dies, indem Sie die Schritte aus AEM Headless-Tutorial befolgen.
>
>  //*********************************

## Anatomie der React-App

Die React-Beispielanwendung besteht aus drei Hauptteilen:

1. Die `src/api` -Ordner enthält Dateien, die für GraphQL-Abfragen an AEM verwendet werden.
   - `src/api/aemHeadlessClient.js` initialisiert und exportiert den AEM Headless-Client, der zur Kommunikation mit AEM verwendet wird
   - `src/api/usePersistedQueries.js` implementiert [benutzerdefinierte React-Hooks](https://react.dev/docs/hooks-custom.html) Daten von AEM GraphQL an die `Teams.js` und `Person.js` Komponenten anzeigen.

1. Die `src/components/Teams.js` zeigt mithilfe einer Listenabfrage eine Liste der Teams und ihrer Mitglieder an.
1. Die `src/components/Person.js` zeigt die Details einer einzelnen Person mithilfe einer parametrisierten Abfrage mit einem Ergebnis an.

## Überprüfen des AEMHeadless-Objekts

Überprüfen Sie die `aemHeadlessClient.js` -Datei für die Erstellung der `AEMHeadless` -Objekt, das zur Kommunikation mit AEM verwendet wird.

1. Öffnen Sie `src/api/aemHeadlessClient.js`.

1. Überprüfen Sie die Zeilen 1-40:

   - Der Import `AEMHeadless` Erklärung [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js), Zeile 11.

   - Die Konfiguration der Autorisierung basierend auf Variablen, die in `.env.development`, Zeile 14-22 und dem Ausdruck für die Pfeilfunktion `setAuthorization`, Zeile 31-40.

   - Die `serviceUrl` -Setup für die enthaltene [Entwicklungs-Proxy](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) Konfiguration, Zeile 27.

1. Die Zeilen 42-49 sind am wichtigsten, da sie die `AEMHeadless` und exportieren Sie es zur Verwendung in der gesamten React-App.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## Implementieren, um AEM von GraphQL persistente Abfragen auszuführen

So implementieren Sie die generische `fetchPersistedQuery(..)` Funktion zum Ausführen der AEM von GraphQL gespeicherten Abfragen öffnen Sie die `usePersistedQueries.js` -Datei. Die `fetchPersistedQuery(..)` -Funktion verwendet die `aemHeadlessClient` -Objekt `runPersistedQuery()` -Funktion, um Abfragen asynchron und versprechen-basiertes Verhalten auszuführen.

Später, benutzerdefinierte React `useEffect` hook ruft diese Funktion auf, um bestimmte Daten aus AEM abzurufen.

1. In `src/api/usePersistedQueries.js` **update** `fetchPersistedQuery(..)`, Zeile 35 mit dem folgenden Code.

```javascript
/**
 * Private, shared function that invokes the AEM Headless client.
 *
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based syntax
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

  // Return the GraphQL and any errors
  return { data, err };
}
```

## Implementieren der Funktionen von Teams

Erstellen Sie als Nächstes die Funktion zum Anzeigen der Teams und ihrer Mitglieder in der Hauptansicht der React-App. Diese Funktion erfordert:

- Eine neue [Benutzerdefinierter React-useEffect-Erweiterungspunkt](https://react.dev/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` , das die `my-project/all-teams` persistente Abfrage, die eine Liste von Team-Inhaltsfragmenten in AEM zurückgibt.
- Eine React-Komponente unter `src/components/Teams.js` , das den neuen benutzerdefinierten React aufruft `useEffect` Erweitern und rendert die Team-Daten.

Nach Abschluss des Vorgangs werden in der Hauptansicht der App die Teamdaten aus AEM angezeigt.

![Team-Ansicht](./assets/graphql-and-external-app/react-app__teams-view.png)

### Schritte

1. Öffnen Sie `src/api/usePersistedQueries.js`.

1. Suchen Sie die Funktion `useAllTeams()`

1. So erstellen Sie eine `useEffect` Erweiterungspunkt, der die persistente Abfrage aufruft `my-project/all-teams` via `fetchPersistedQuery(..)`, fügen Sie den folgenden Code hinzu. Der Erweiterungspunkt gibt auch nur die relevanten Daten aus der AEM GraphQL-Antwort unter `data?.teamList?.items`, sodass die React-Ansicht-Komponenten unabhängig von den übergeordneten JSON-Strukturen sein können.

   ```javascript
   /**
    * Custom hook that calls the 'my-project/all-teams' persisted query.
    *
    * @returns an array of Team JSON objects, and array of errors
    */
   export function useAllTeams() {
     const [teams, setTeams] = useState(null);
     const [error, setError] = useState(null);
   
     // Use React useEffect to manage state changes
     useEffect(() => {
       async function fetchData() {
         // Call the AEM GraphQL persisted query named "my-project/all-teams"
         const { data, err } = await fetchPersistedQuery(
           "my-project/all-teams"
         );
         // Sets the teams variable to the list of team JSON objects
         setTeams(data?.teamList?.items);
         // Set any errors
         setError(err);
       }
       // Call the internal fetchData() as per React best practices
       fetchData();
     }, []);
   
     // Returns the teams and errors
     return { teams, error };
   }
   ```

1. Öffnen Sie `src/components/Teams.js`

1. Im `Teams` React-Komponente: Rufen Sie die Liste der Teams aus AEM mithilfe der `useAllTeams()` Haken.

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. Führen Sie die ansichtsbasierte Datenvalidierung durch, zeigen Sie eine Fehlermeldung an oder laden Sie die Anzeige basierend auf den zurückgegebenen Daten.

   ```javascript
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       // If an error ocurred while executing the GraphQL query, display an error message
       return <Error errorMessage={error} />;
     } else if (!teams) {
       // While the GraphQL request is executing, show the Loading indicator
       return <Loading />;
     }
     ...
   }
   ```

1. Rendern Sie schließlich die Team-Daten. Jedes von der GraphQL-Abfrage zurückgegebene Team wird mithilfe der bereitgestellten `Team` React-Unterkomponente.

   ```javascript
   import React from "react";
   import { Link } from "react-router-dom";
   import { useAllTeams } from "../api/usePersistedQueries";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Teams.scss";
   
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!teams) {
       return <Loading />;
     }
   
     // Teams have been populated by AEM GraphQL query. Display the teams.
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return <Team key={index} {...team} />;
         })}
       </div>
     );
   }
   
   // Render single Team
   function Team({ title, shortName, description, teamMembers }) {
     // Must have title, shortName and at least 1 team member
     if (!title || !shortName || !teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{title}</h2>
         <p className="team__description">{description.plaintext}</p>
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render the referenced Person models associated with the team */}
             {teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember.fullName}`}>
                     {teamMember.fullName}
                   </Link>
                 </li>
               );
             })}
           </ul>
         </div>
       </div>
     );
   }
   
   export default Teams;
   ```


## Funktion &quot;Person implementieren&quot;

Mit dem [Team-Funktionen](#implement-teams-functionality) abschließen, lassen Sie uns die Funktion implementieren, um die Anzeige in den Details eines Teammitglieds oder einer Person zu verarbeiten.

Diese Funktion erfordert:

- Eine neue [Benutzerdefinierter React-useEffect-Erweiterungspunkt](https://react.dev/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` , der die parametrisierte `my-project/person-by-name` persistente Abfrage und gibt einen Datensatz für eine einzelne Person zurück.

- Eine React-Komponente unter `src/components/Person.js` , der den vollständigen Namen einer Person als Abfrageparameter verwendet, ruft die neue benutzerdefinierte React auf `useEffect` Erweitern und rendert die Personendaten.

Wenn Sie nach Abschluss des Vorgangs den Namen einer Person in der Ansicht &quot;Teams&quot;auswählen, wird die Personenansicht angezeigt.

![Person](./assets/graphql-and-external-app/react-app__person-view.png)

1. Öffnen Sie `src/api/usePersistedQueries.js`.

1. Suchen Sie die Funktion `usePersonByName(fullName)`

1. So erstellen Sie eine `useEffect` Erweiterungspunkt, der die persistente Abfrage aufruft `my-project/all-teams` via `fetchPersistedQuery(..)`, fügen Sie den folgenden Code hinzu. Der Erweiterungspunkt gibt auch nur die relevanten Daten aus der AEM GraphQL-Antwort unter `data?.teamList?.items`, sodass die React-Ansicht-Komponenten unabhängig von den übergeordneten JSON-Strukturen sein können.

   ```javascript
   /**
    * Calls the 'my-project/person-by-name' and provided the {fullName} as the persisted query's `name` parameter.
    *
    * @param {String!} fullName the full
    * @returns a JSON object representing the person
    */
   export function usePersonByName(fullName) {
     const [person, setPerson] = useState(null);
     const [errors, setErrors] = useState(null);
   
     useEffect(() => {
       async function fetchData() {
         // The key is the variable name as defined in the persisted query, and may not match the model's field name
         const queryParameters = { name: fullName };
   
         // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
         const { data, err } = await fetchPersistedQuery(
           "my-project/person-by-name",
           queryParameters
         );
   
         if (err) {
           // Capture errors from the HTTP request
           setErrors(err);
         } else if (data?.personList?.items?.length === 1) {
           // Set the person data after data validation
           setPerson(data.personList.items[0]);
         } else {
           // Set an error if no person could be found
           setErrors(`Cannot find person with name: ${fullName}`);
         }
       }
       fetchData();
     }, [fullName]);
   
     return { person, errors };
   }
   ```

1. Öffnen Sie `src/components/Person.js`
1. Im `Person` React-Komponente, analysieren Sie die `fullName` Route-Parameter und rufen Sie die Personendaten mithilfe der AEM ab. `usePersonByName(fullName)` Haken.

   ```javascript
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   ...
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
     ...
   }
   ```

1. Führen Sie eine ansichtsbasierte Datenvalidierung durch, zeigen Sie eine Fehlermeldung an oder laden Sie einen Indikator basierend auf den zurückgegebenen Daten.

   ```javascript
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
     ...
   }
   ```

1. Geben Sie schließlich die Personendaten wieder.

   ```javascript
   import React from "react";
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   import { mapJsonRichText } from "../utils/renderRichText";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Person.scss";
   
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
   
     // Render the person data
     return (
       <div className="person">
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI+person.profilePicture._path}
           alt={person.fullName}
         />
         <div className="person__occupations">
           {person.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
         <div className="person__content">
           <h1 className="person__full-name">{person.fullName}</h1>
           <div className="person__biography">
             {/* Use this utility to transform multi-line text JSON into HTML */}
             {mapJsonRichText(person.biographyText.json)}
           </div>
         </div>
       </div>
     );
   }
   
   export default Person;
   ```

## Testen der App

Überprüfen der App [http://localhost:3000/](http://localhost:3000/) und klicken Sie auf _Mitglieder_ Links. Außerdem können Sie dem Team-Alpha weitere Teams und/oder Mitglieder hinzufügen, indem Sie Inhaltsfragmente in AEM hinzufügen.

## Unterhalb des

Öffnen Sie die **Entwicklertools** > **Netzwerk** und _Filter_ für `all-teams` -Anfrage. Beachten Sie die GraphQL-API-Anfrage. `/graphql/execute.json/my-project/all-teams` gegen `http://localhost:3000` und **NOT** gegen den Wert von `REACT_APP_HOST_URI` (z. B. <https://publish-p123-e456.adobeaemcloud.com>). Die Anfragen werden für die Domäne der React-App gestellt, weil [Proxy-Einrichtung](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) aktiviert wird, wenn `http-proxy-middleware` -Modul.


![GraphQL-API-Anfrage über Proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Überprüfen Sie die wichtigsten `../setupProxy.js` Datei und innerhalb `../proxy/setupProxy.auth.**.js` -Dateien erkennen, wie `/content` und `/graphql` Pfade werden proximiert und weisen darauf hin, dass es sich nicht um ein statisches Asset handelt.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

Die Verwendung des lokalen Proxys ist keine geeignete Option für die Produktionsbereitstellung. Weitere Informationen finden Sie unter _Produktionsbereitstellung_ Abschnitt.

## Herzlichen Glückwunsch!{#congratulations}

Herzlichen Glückwunsch! Sie haben die React-App erfolgreich erstellt, um Daten aus AEM GraphQL-APIs als Teil des grundlegenden Tutorials zu nutzen und anzuzeigen!
