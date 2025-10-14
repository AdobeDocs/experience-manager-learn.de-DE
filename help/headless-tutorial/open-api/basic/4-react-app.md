---
title: Erstellen einer React-App mit AEM OpenAPI | Headless-Tutorial, Teil 4
description: Erste Schritte mit Adobe Experience Manager (AEM) und OpenAPI. Erstellen Sie eine React-App, die Inhalte/Daten aus den OpenAPI-basierten APIs zur Bereitstellung von Inhaltsfragmenten in AEM abruft.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 900
exl-id: ced704d4-5098-4553-812f-9734da0c5777
source-git-commit: e7960fa75058c072b1b52ba1a0d7c99a0280d02f
workflow-type: ht
source-wordcount: '874'
ht-degree: 100%

---

# Erstellen einer React-App mit OpenAPIs zur Bereitstellung von Inhaltsfragmenten in AEM

In diesem Kapitel erfahren Sie, wie die Bereitstellung von AEM-Inhaltsfragmenten mit APIs von OpenAPI das Erlebnis in externen Programmen verbessern kann.

Eine einfache React-App wird zum Abfragen und Anzeigen von **Team**- und **Personen**-Inhalten verwendet, die von AEM mit APIs von OpenAPI bereitgestellt werden. Die Verwendung von React ist größtenteils unwichtig und die externe Anwendung, die es verwendet, könnte in jedem Framework für jede Plattform erstellt werden, sofern sie HTTP-Anfragen an AEM as a Cloud Service senden kann.

## Voraussetzungen

Es wird vorausgesetzt, dass die in den vorherigen Teilen dieses mehrteiligen Tutorials beschriebenen Schritte abgeschlossen wurden.

Die folgende Software muss installiert sein:

* [Node.js v22+](https://nodejs.org/de)
* [Visual Studio Code](https://code.visualstudio.com/)

## Ziele

Erfahren Sie mehr über:

* Herunterladen und Starten der als Beispiel dienenden React-App.
* Aufrufen der Bereitstellung von AEM-Inhaltsfragmenten mit APIs von OpenAPI für eine Liste von Teams und deren referenzierten Mitgliedern.
* Aufrufen der Bereitstellung von AEM-Inhaltsfragmenten mit APIs von OpenAPI, um die Details eines Team-Mitglieds abzurufen.

## Einrichten von CORS in AEM as a Cloud Service

Diese Beispiel-React-App wird lokal (auf `http://localhost:3000`) ausgeführt und stellt eine Verbindung zur Bereitstellung von AEM-Inhaltsfragmenten durch den AEM-Veröffentlichungs-Service mit APIs von OpenAPI her. Um diese Verbindung zuzulassen, muss CORS (Cross-Origin Resource Sharing) im Veröffentlichungs- (oder Vorschau-) Service von AEM konfiguriert werden.

Befolgen Sie die [Anweisungen zum Einrichten einer auf `http://localhost:3000` ausgeführten SPA, um CORS-Anfragen an den AEM-Veröffentlichungs-Service zuzulassen](https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#different-domains).

### Lokaler CORS-Proxy

Alternativ können Sie zur Entwicklung einen [lokalen CORS-Proxy](https://www.npmjs.com/package/local-cors-proxy) ausführen, der eine CORS-freundliche Verbindung zu AEM bereitstellt.

```bash
$ npm install --global lcp
$ lcp --proxyUrl https://publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com
```

Aktualisieren Sie den Wert `--proxyUrl` auf Ihre AEM-Veröffentlichungs- (oder Vorschau-) URL.

Wenn der lokale CORS-Proxy ausgeführt wird, greifen Sie unter `http://localhost:8010/proxy` auf die AEM-APIs zur Bereitstellung von Inhaltsfragmenten zu, um CORS-Probleme zu vermeiden.

## Klonen der Beispiel-React-App

Eine abgespeckte Beispiel-React-App wird mit dem Code implementiert, der für die Interaktion mit der Bereitstellung von AEM-Inhaltsfragmenten mit APIs von OpenAPI erforderlich ist, und zeigt die von ihnen erhaltenen Team- und Personendaten an.

Der Beispiel-Quell-Code der React-App ist [auf Github.com verfügbar](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic).

So rufen Sie die React-App ab:

1. Klonen Sie die als Beispiel gegebene OpenAPI-React-App von WKND unter [Github.com](https://github.com/adobe/aem-tutorials) im [`headless_open-api_basic`-Tag](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-tutorials.git
   $ cd aem-tutorials  
   $ git fetch --tags
   $ git tag
   $ git checkout tags/headless_open-api_basic
   ```

1. Navigieren Sie zum Ordner `headless/open-api/basic` und öffnen Sie ihn in Ihrer IDE.

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ code .
   ```

1. Aktualisieren Sie `.env`, um eine Verbindung zum Veröffentlichungs-Service von AEM as a Cloud Service herzustellen, da hier unsere Inhaltsfragmente veröffentlicht werden. Sie können auf den AEM-Vorschau-Service verweisen, wenn Sie die App mit dem AEM-Vorschau-Service testen möchten (und die Inhaltsfragmente dort veröffentlicht werden).

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   ```

   Wenn Sie [Lokaler CORS-Proxy](#local-cors-proxy) verwenden, setzen Sie `REACT_APP_HOST_URI` auf `http://localhost:8010/proxy`.

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=http://localhost:8010/proxy
   ```

1. Starten der React-App

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ npm install
   $ npm start
   ```

1. Die React-App startet im Entwicklungsmodus auf [http://localhost:3000/](http://localhost:3000/). Änderungen, die im Laufe des Tutorials an der React-App vorgenommen wurden, werden im Webbrowser sofort berücksichtigt.

>[!IMPORTANT]
>
>   Diese React-App ist teilweise implementiert. Führen Sie die Schritte in diesem Tutorial aus, um die Implementierung abzuschließen. Die JavaScript-Dateien, für die Implementierungsarbeiten erforderlich sind, enthalten den folgenden Kommentar: Stellen Sie sicher, dass Sie dem Code in diesen Dateien den in diesem Tutorial angegebenen Code hinzufügen bzw. ihn aktualisieren.
>
>
>  //*********************************
>  >  // TODO: Implementieren Sie dies, indem Sie die Schritte aus dem AEM Headless-Tutorial befolgen
>  >  //*********************************
>

## Anatomie der React-App

Die React-App im Beispiel besteht aus drei Hauptteilen, die aktualisiert werden müssen:

1. Die `.env`-Datei enthält die URL des AEM-Veröffentlichungs- (oder Vorschau-) Service.
1. `src/components/Teams.js` zeigt eine Liste der Teams und ihrer Mitglieder an.
1. `src/components/Person.js` zeigt die Details eines einzelnen Team-Mitglieds an.

## Implementieren der Teams-Funktionalität

Erstellen Sie die Funktionalität zum Anzeigen der Teams und ihrer Mitglieder in der Hauptansicht der React-App. Diese Funktionalität erfordert Folgendes:

* Einen neuen [benutzerdefinierten React-Hook „useEffect“](https://react.dev/reference/react/useEffect#useeffect), der die API **Alle Inhaltsfragmente auflisten** über eine Abrufanfrage aufruft und dann den Wert `fullName` für jedes `teamMember` zur Anzeige abruft.

Nach Abschluss des Vorgangs werden in der Hauptansicht der App die Team-Daten aus AEM angezeigt.

![Team-Ansicht](./assets/4/teams.png)

1. Öffnen Sie `src/components/Teams.js`.

1. Implementieren Sie die **Teams**-Komponente, um die Liste der Teams aus der API [Alle Inhaltsfragmente auflisten](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragments) abzurufen und die Team-Inhalte zu rendern. Dies ist in die folgenden Schritte unterteilt:

1. Erstellen Sie einen `useEffect`-Hook, der die API **Alle Inhaltsfragmente auflisten** von AEM aufruft und die Daten im Status der React-Komponente speichert.
1. Rufen Sie für jedes **Team**-Inhaltsfragment die API **Inhaltsfragment abrufen** auf, um die vollständig hydrierten Details des Teams abzurufen, einschließlich der Mitglieder und ihrer `fullNames`.
1. Rendern Sie die Team-Daten mit der `Team`-Funktion.

   ```javascript
   import { useEffect, useState } from "react";
   import { Link } from "react-router-dom";
   import "./Teams.scss";
   
   function Teams() {
   
     // The teams folder is the only folder-tree that is allowed to contain Team Content Fragments.
     const TEAMS_FOLDER = '/content/dam/my-project/en/teams';
   
     // State to store the teams data
     const [teams, setTeams] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches all teams and their associated member details
       * This is a two-step process:
       * 1. First, get all team content fragments from the specified folder
       * 2. Then, for each team, fetch the full details including hydrated references to get the team member names
       */
       const fetchData = async () => {
         try {
           // Step 1: Fetch all teams from the teams folder
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments?path=${TEAMS_FOLDER}`
           );
           const allTeams = (await response.json()).items || [];
   
           // Step 2: Fetch detailed information for each team with hydrated references
           const hydratedTeams = [];
           for (const team of allTeams) {
             const hydratedTeamResponse = await fetch(
               `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${team.id}?references=direct-hydrated`
             );
             hydratedTeams.push(await hydratedTeamResponse.json());
           }
   
           setTeams(hydratedTeams);
         } catch (error) {
           console.error("Error fetching content fragments:", error);
         }
       };
   
       fetchData();
     }, [TEAMS_FOLDER]);
   
     // Show loading state while teams data is being fetched
     if (!teams) {
       return <div>Loading teams...</div>;
     }
   
     // Render the teams
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return (
             <Team 
               key={index} 
               {..team}
             />
           );
         })}
       </div>
     );
   }
   
   /**
   * Team component - renders a single team with its details and members
   * @param {string} fields - The authorable fields
   * @param {Object} references - Hydrated references containing member details such as fullName
   */
   function Team({ fields, references, path }) {
     if (!fields.title || !fields.teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{fields.title}</h2>
         {/* Render description as HTML using dangerouslySetInnerHTML */}
         <p 
           className="team__description" 
           dangerouslySetInnerHTML={{ __html: fields.description.value }}
         />
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render each team member as a link to their detail page */}
             {fields.teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember}`}>
                     {/* Display the full name from the hydrated reference */}
                     {references[teamMember].value.fields.fullName}
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

Nachdem die [Teams-Funktionalität](#implement-teams-functionality) nun erstellt ist, implementieren Sie die Funktionalität zur Anzeige der Details einzelner Team-Mitglieder oder Personen.

![Personen-Ansicht](./assets/4/person.png)

Gehen Sie hierfür wie folgt vor:

1. Öffnen Sie `src/components/Person.js`
1. Analysieren Sie in der React-Komponente `Person` den Routenparameter `id`. Beachten Sie, dass die Routen der React-App zuvor so eingerichtet waren, dass sie den URL-Parameter `id` akzeptierten (siehe `/src/App.js`).
1. Rufen Sie die Personendaten über die API [Inhaltsfragment abrufen](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragment) aus AEM ab.

   ```javascript
   import "./Person.scss";
   import { useEffect, useState } from "react";
   import { useParams } from "react-router-dom";
   
   /**
   * Person component - displays detailed information about a single person
   * Fetches person data from AEM using the ID from the URL parameters
   */
   function Person() {
     // Get the person ID from the URL parameter
     const { id } = useParams();
   
     // State to store the person data
     const [person, setPerson] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches person data from AEM Content Fragment Delivery API
       * Uses the ID from URL parameters to get the specific person's details
       */
       const fetchData = async () => {
         try {
           /* Hydrate references for access to profilePicture asset path */
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${id}?references=direct-hydrated`
           );
           const json = await response.json();
           setPerson(json || null);
         } catch (error) {
           console.error("Error fetching person data:", error);
         }
       };
       fetchData();
     }, [id]); // Re-fetch when ID changes
   
     // Show loading state while person data is being fetched
     if (!person) {
       return <div>Loading person...</div>;
     }
   
     return (
       <div className="person">
         {/* Person profile image - Look up the profilePicture reference in the references object */}
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI + person.references[person.fields.profilePicture].value.path}
           alt={person.fields.fullName}
         />
         {/* Display person's occupations */}
         <div className="person__occupations">
           {person.fields.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
   
         {/* Person's main content: name and biography */}
         <div className="person__content">
           <h1 className="person__full-name">{person.fields.fullName}</h1>
           {/* Render biography as HTML content */}
           <div
             className="person__biography"
             dangerouslySetInnerHTML={{ __html: person.fields.biographyText.value }}
           />
         </div>
       </div>
     );  
   }
   
   export default Person;
   ```

### Abrufen des vollständigen Codes

Der vollständige Quell-Code für dieses Kapitel ist [auf Github.com verfügbar](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end).

```bash
$ git fetch --tags
$ git tag
$ git checkout tags/headless_open-api_basic_4-end
```

## Testen der App

Prüfen Sie die App [http://localhost:3000/](http://localhost:3000/) und klicken Sie auf die Links _Team-Mitglied_. Außerdem können Sie dem Team-Alpha weitere Teams und/oder Mitglieder hinzufügen, indem Sie Inhaltsfragmente im AEM-Autoren-Service hinzufügen und veröffentlichen.

## Im Hintergrund

Öffnen Sie im Browser die Konsole **Entwickler-Tools > Netzwerk** und **Filtern** Sie nach `/adobe/contentFragments`-Abrufanfragen, während Sie mit der React-App interagieren.

## Herzlichen Glückwunsch!

Herzlichen Glückwunsch! Sie haben erfolgreich eine React-App erstellt, um Inhaltsfragmente aus der AEM-Inhaltsfragmentbereitstellung mit APIs von OpenAPI zu nutzen und anzuzeigen.
