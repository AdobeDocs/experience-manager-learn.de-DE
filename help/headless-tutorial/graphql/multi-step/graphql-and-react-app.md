---
title: Erstellen einer React-App, die AEM mit GraphQL API abfragt – Erste Schritte mit AEM Headless – GraphQL
description: Erste Schritte mit Adobe Experience Manager (AEM) und GraphQL. Erstellen Sie eine React-App, die Inhalte/Daten aus AEM GraphQL-API abruft. Außerdem sehen Sie, wie das AEM Headless-JS-SDK verwendet wird.
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 83%

---


# Erstellen einer React-App, die AEM GraphQL-APIs verwendet

In diesem Kapitel erfahren Sie, wie AEM GraphQL-APIs das Erlebnis in einer externen Anwendung steuern können.

Eine einfache React-App wird zum Abfragen und Anzeigen von **Team**- und **Personen**-Inhalten verwendet, die von AEM GraphQL-APIs bereitgestellt werden. Die Verwendung von React ist größtenteils unwichtig, und die externe Anwendung, die es nutzt, könnte in jedem Rahmen für jede Plattform geschrieben werden.

## Voraussetzungen

Es wird davon ausgegangen, dass die in den vorherigen Teilen dieses mehrteiligen Tutorials beschriebenen Schritte abgeschlossen wurden, oder [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip) wird auf Ihrem AEM as a Cloud Service Autoren- und Veröffentlichungsdienst installiert.

_IDE-Screenshots in diesem Kapitel stammen von [Visual Studio Code](https://code.visualstudio.com/)_

Die folgende Software muss installiert sein:

- [Node.js v18](https://nodejs.org/de)
- [Visual Studio Code](https://code.visualstudio.com/)

## Ziele

Erfahren Sie mehr über:

- Herunterladen und Starten der als Beispiel dienenden React-App
- Abfragen von AEM GraphQL-Endpunkten mithilfe der [AEM Headless-JS-SDK](https://github.com/adobe/aem-headless-client-js)
- Abfragen von AEM für eine Liste von Teams und deren referenzierter Mitglieder
- Abfrage von AEM nach den Details eines Team-Mitglieds

## Abrufen der Beispiel-React-App

In diesem Kapitel wird eine abgespeckte Beispiel-React-App implementiert, die den Code enthält, der für die Interaktion mit der AEM GraphQL-API erforderlich ist, und die Teams- und Personendaten anzeigt, die von dort erfasst wurden.

Der Beispiel-Quell-Code der React-App ist auf Github.com unter <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial> verfügbar

So rufen Sie die React-App ab:

1. Klonen Sie die als Beispiel gegebene WKND GraphQL-React-App von [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navigieren Sie zum Ordner `basic-tutorial` und öffnen Sie ihn in Ihrer IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![React-App in VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Aktualisieren Sie `.env.development`, um eine Verbindung zum Veröffentlichungs-Service von AEM as a Cloud Service herzustellen.

   - Legen Sie den Wert von `REACT_APP_HOST_URI` auf die Veröffentlichungs-URL Ihres AEM as a Cloud Service fest (z. B. `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) und den Wert von `REACT_APP_AUTH_METHOD` auf `none`

   >[!NOTE]
   >
   > Vergewissern Sie sich, dass Sie die Projektkonfiguration, Inhaltsfragmentmodelle, erstellten Inhaltsfragmente, GraphQL-Endpunkte und persistierten Abfragen aus den vorherigen Schritten veröffentlicht haben.
   >
   > Wenn Sie die obigen Schritte auf dem lokalen AEM Author-SDK durchgeführt haben, können Sie auf `http://localhost:4502` verweisen und den Wert von `REACT_APP_AUTH_METHOD` auf `basic` setzen.


1. Wechseln Sie in der Befehlszeile zum Ordner `aem-guides-wknd-graphql/basic-tutorial`

1. Starten der React-App

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. Die React-App startet im Entwicklungsmodus auf [http://localhost:3000/](http://localhost:3000/). Änderungen, die während des Tutorials an der React-App vorgenommen wurden, werden sofort übernommen.

![Teilweise implementierte React-App](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Diese React-App ist teilweise implementiert. Führen Sie die Schritte in diesem Tutorial aus, um die Implementierung abzuschließen. Die JavaScript-Dateien, für die Implementierungsarbeiten erforderlich sind, enthalten den folgenden Kommentar: Stellen Sie sicher, dass Sie dem Code in diesen Dateien den in diesem Tutorial angegebenen Code hinzufügen bzw. ihn aktualisieren.
>
>
> //*********************************
>
>  // TODO Implementieren Sie dies, indem Sie die Schritte aus AEM Headless-Tutorial befolgen.
>
>  //*********************************
>

## Anatomie der React-App

Die React-App im Beispiel besteht aus drei Hauptteilen:

1. Der Ordner `src/api` enthält Dateien, die zur Erstellung von GraphQL-Abfragen an AEM verwendet werden.
   - `src/api/aemHeadlessClient.js` initialisiert und exportiert den AEM Headless-Client, der zur Kommunikation mit AEM verwendet wird
   - `src/api/usePersistedQueries.js` implementiert [angepasste React-Hooks](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components), die Daten von AEM GraphQL an die Ansichtskomponenten `Teams.js` und `Person.js` zurückgeben.

1. `src/components/Teams.js` zeigt mithilfe einer Listenabfrage eine Liste der Teams und ihrer Mitglieder an.
1. Die Datei `src/components/Person.js` zeigt die Details einer einzelnen Person mithilfe einer parametrisierten Abfrage mit einem Ergebnis an.

## Überprüfen des AEM Headless-Objekts

Die Datei `aemHeadlessClient.js` enthält Informationen zum Erstellen des zur Kommunikation mit AEM verwendeten `AEMHeadless`-Objekts.

1. Öffnen Sie `src/api/aemHeadlessClient.js`.

1. Sehen Sie sich die Zeilen 1–40 an:

   - Die `AEMHeadless`-Importantweisung vom [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js), Zeile 11.

   - Die Autorisierungskonfiguration basierend auf den in `.env.development` definierten Variablen, Zeile 14-22, und dem Ausdruck für die Pfeilfunktion `setAuthorization`, Zeile 31–40.

   - Das `serviceUrl`-Setup für die enthaltene [Entwicklungs-Proxy](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests)-Konfiguration, Zeile 27.

1. Die Zeilen 42–49 sind am wichtigsten, da sie den `AEMHeadless`-Client instanziieren und zur Verwendung in der gesamten React-App exportieren.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## Implementieren zum Ausführen von AEM GraphQL-persistierten Abfragen

So implementieren Sie die generische `fetchPersistedQuery(..)` Funktion zum Ausführen der AEM von GraphQL gespeicherten Abfragen öffnen Sie die `usePersistedQueries.js` -Datei. Die Funktion `fetchPersistedQuery(..)` verwendet die Funktion `runPersistedQuery()` des `aemHeadlessClient`-Objekts, um Abfragen asynchron auszuführen und versprechensbasiertes Verhalten zu verwenden.

Später wird diese Funktion durch den benutzerdefinierten `useEffect`-React-Hook aufgerufen, um bestimmte Daten aus AEM abzurufen.

1. Führen Sie in `src/api/usePersistedQueries.js` eine **Aktualisierung** von `fetchPersistedQuery(..)`, Zeile 35, mit folgendem Code durch:

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

## Implementieren der Teams-Funktionalität

Erstellen Sie als Nächstes die Funktionalität zum Anzeigen der Teams und ihrer Mitglieder in der Hauptansicht der React-App. Diese Funktionalität erfordert Folgendes:

- einen neuen [benutzerdefinierten useEffect-React-Hook](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js`, der die `my-project/all-teams`-persistierte Abfrage aufruft und eine Liste von Team-Inhaltsfragmenten in AEM zurückgibt,
- eine React-Komponente unter `src/components/Teams.js`, die den neuen benutzerdefinierten `useEffect`-React-Hook aufruft und die Team-Daten rendert.

Nach Abschluss des Vorgangs werden in der Hauptansicht der App die Team-Daten aus AEM angezeigt.

![Team-Ansicht](./assets/graphql-and-external-app/react-app__teams-view.png)

### Schritte

1. Öffnen Sie `src/api/usePersistedQueries.js`.

1. Suchen Sie die Funktion `useAllTeams()`.

1. Um einen `useEffect`-Hook zu erstellen, der die persistierte Abfrage `my-project/all-teams` über `fetchPersistedQuery(..)` aufruft, fügen Sie den folgenden Code hinzu. Der Hook gibt außerdem nur die relevanten Daten aus der AEM GraphQL-Antwort unter `data?.teamList?.items` zurück, sodass die React-Ansichtskomponenten unabhängig von den übergeordneten JSON-Strukturen sein können.

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

1. Rufen Sie in der React-Komponente `Teams` mithilfe des Hooks `useAllTeams()` die Liste der Teams aus AEM ab.

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. Führen Sie die ansichtsbasierte Datenvalidierung durch, wobei entweder eine Fehlermeldung oder Ladeanzeige basierend auf den zurückgegebenen Daten angezeigt wird.

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

1. Rendern Sie schließlich die Team-Daten. Jedes von der GraphQL-Abfrage zurückgegebene Team wird mithilfe der bereitgestellten React-Unterkomponente `Team` gerendert.

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


## Implementieren der Personen-Funktionalität

Lassen Sie uns nun, nach Fertigstellung der [Team-Funktionalität](#implement-teams-functionality), die Funktionalität zur Anzeige der Details von einzelnen Team-Mitgliedern oder Personen implementieren.

Diese Funktionalität erfordert Folgendes:

- einen neuen [benutzerdefiniertern useEffect-React-Hook](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js`, der die parametrisierte `my-project/person-by-name`-persistierte Abfrage aufruft und den Datensatz für eine einzelne Person zurückgibt,

- eine React-Komponente unter `src/components/Person.js`, die den vollständigen Namen einer Person als Abfrageparameter verwendet, den neuen benutzerdefinierten `useEffect`-React-Hook aufruft und die Personendaten rendert.

Wenn Sie nach Abschluss des Vorgangs den Namen einer Person in der Team-Ansicht auswählen, wird die Personenansicht gerendert.

![Person](./assets/graphql-and-external-app/react-app__person-view.png)

1. Öffnen Sie `src/api/usePersistedQueries.js`.

1. Suchen Sie die Funktion `usePersonByName(fullName)`.

1. Um einen `useEffect`-Hook zu erstellen, der die persistierte Abfrage `my-project/all-teams` über `fetchPersistedQuery(..)` aufruft, fügen Sie den folgenden Code hinzu. Der Hook gibt außerdem nur die relevanten Daten aus der AEM GraphQL-Antwort unter `data?.teamList?.items` zurück, sodass die React-Ansichtskomponenten unabhängig von den übergeordneten JSON-Strukturen sein können.

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
1. Analysieren Sie in der React-Komponente `Person` den Routenparameter `fullName` und rufen Sie die Personendaten mithilfe des `usePersonByName(fullName)`-Hooks aus AEM ab.

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

1. Führen Sie eine ansichtsbasierte Datenvalidierung durch, wobei eine Fehlermeldung oder Ladeanzeige basierend auf den zurückgegebenen Daten angezeigt wird.

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

1. Rendern Sie schließlich die Personendaten.

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

Sehen Sie sich die App unter [http://localhost:3000/](http://localhost:3000/) an und klicken Sie auf die Links _Mitglieder_. Außerdem können Sie dem Team-Alpha weitere Teams und/oder Mitglieder hinzufügen, indem Sie Inhaltsfragmente in AEM hinzufügen.

>[!IMPORTANT]
>
>Wenn Sie Ihre Implementierungsänderungen überprüfen möchten oder die App nach den oben genannten Änderungen nicht funktionsfähig ist, lesen Sie den Abschnitt [Grundlegendes Tutorial](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) Lösungsverzweigung.

## Unter der Haube

Öffnen Sie die **Entwicklertools** > **Netzwerk** und _Filter_ für `all-teams` -Anfrage. Beachten Sie die GraphQL-API-Anfrage `/graphql/execute.json/my-project/all-teams` gegen `http://localhost:3000` und **NOT** gegen den Wert von `REACT_APP_HOST_URI`, beispielsweise `<https://publish-pxxx-exxx.adobeaemcloud.com`. Die Anfragen werden für die Domäne der React-App gestellt, weil [Proxy-Einrichtung](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) aktiviert wird, wenn `http-proxy-middleware` -Modul.


![GraphQL-API-Anfrage über Proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Sehen Sie sich die Hauptdatei `../setupProxy.js` an. Innerhalb der Dateien `../proxy/setupProxy.auth.**.js` fällt auf, dass die Pfade `/content` und `/graphql` Pfade als Proxy verwendet werden. Zudem ist angegeben, dass es sich nicht um ein statisches Asset handelt.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

Die Verwendung des lokalen Proxys ist keine geeignete Option für die Produktionsbereitstellung. Weitere Informationen finden Sie unter _Produktionsbereitstellung_ Abschnitt.

## Herzlichen Glückwunsch!{#congratulations}

Herzlichen Glückwunsch! Sie haben die React-App erfolgreich erstellt, um Daten aus AEM GraphQL-APIs als Teil des grundlegenden Tutorials zu nutzen und anzuzeigen.
