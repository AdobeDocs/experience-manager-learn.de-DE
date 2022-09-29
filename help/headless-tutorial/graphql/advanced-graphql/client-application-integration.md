---
title: Client-Anwendungsintegration - Erweiterte Konzepte von AEM Headless - GraphQL
description: Implementieren Sie persistente Abfragen und integrieren Sie sie in die WKND-App.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 2%

---

# Client-Anwendungsintegration

Im vorherigen Kapitel haben Sie persistente Abfragen mithilfe von HTTP-PUT- und POST-Anfragen erstellt und aktualisiert.

Dieses Kapitel führt Sie durch die Schritte, die zur Integration dieser persistenten Abfragen in die WKND-App mithilfe von HTTP-GET in fünf React-Komponenten erforderlich sind:

* Speicherort
* Adresse
* Instructor
* Administrator
* Team

## Voraussetzungen {#prerequisites}

Dieses Dokument ist Teil eines mehrteiligen Tutorials. Bevor Sie mit diesem Kapitel fortfahren, vergewissern Sie sich bitte, dass die vorherigen Kapitel abgeschlossen sind. Abschluss der [Grundlegendes Tutorial](/help/headless-tutorial/graphql/multi-step/overview.md) wird empfohlen.

_IDE-Screenshots in diesem Kapitel stammen von [Visual Studio-Code](https://code.visualstudio.com/)_

### Kapitel 1-4 Lösungspaket (optional) {#solution-package}

Es ist ein Lösungspaket verfügbar, das installiert wird, das die Schritte in der AEM Benutzeroberfläche für die Kapitel 1-4 abschließt. Dieses Paket ist **nicht erforderlich** wenn die vorherigen Kapitel abgeschlossen sind.

1. Download [Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip).
1. Navigieren Sie AEM zu **Instrumente** > **Implementierung** > **Pakete** für den Zugriff **Package Manager**.
1. Laden Sie das im vorherigen Schritt heruntergeladene Paket (ZIP-Datei) hoch und installieren Sie es.

## Ziele {#objectives}

In diesem Tutorial erfahren Sie, wie Sie die Anforderungen für persistente Abfragen mit dem AEM Headless-JavaScript in die WKND GraphQl React-Beispielanwendung integrieren. [SDK](https://github.com/adobe/aem-headless-client-js).

## Installieren und Ausführen der Beispiel-Clientanwendung {#install-client-app}

Um das Tutorial zu beschleunigen, wird eine Starter-React-JS-App bereitgestellt.

>[!NOTE]
> 
> Nachfolgend finden Sie Anweisungen zum Verbinden der React-App mit einem **Autor** -Umgebung in AEM as a Cloud Service mithilfe einer [Zugriffstoken für lokale Entwicklung](/help/headless-tutorial/authentication/local-development-access-token.md). Es ist auch möglich, die App mit einem [lokale Autoreninstanz mit dem AEMaaCS-SDK](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) einfache Authentifizierung verwenden.

1. Download **[aem-guides-wknd-headless-start-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-start-tutorial.zip)**.
1. Entpacken Sie die Datei und öffnen Sie das Projekt in Ihrer IDE.
1. Abrufen einer [Lokales Entwicklungstoken](/help/headless-tutorial/authentication/local-development-access-token.md) für Ihre AEM.
1. Öffnen Sie im Projekt die Datei . `.env.development`.
   1. Satz `REACT_APP_DEV_TOKEN` gleich `accessToken` aus dem lokalen Entwicklungstoken. (Nicht die gesamte JSON-Datei)
   1. Satz `REACT_APP_HOST_URI` an die URL Ihres AEM **Autor** Umgebung.

   ![Lokale Umgebungsdatei aktualisieren](assets/client-application-integration/update-environment-file.png)
1. Öffnen Sie ein neues Terminal und navigieren Sie zum Projektordner. Führen Sie folgende Befehle aus:

   ```shell
   $ npm install
   $ npm start
   ```

1. Ein neuer Browser sollte unter `http://localhost:3000/aem-guides-wknd-pwa`.
1. Tippen **Campingplatz** > **Yosemite Backpacken** , um die Yosemite Backpacking Abenteuerdetails zu sehen.

   ![Yosemite-Backpacker-Bildschirm](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Öffnen Sie die Entwicklertools des Browsers und überprüfen Sie die `XHR` Anfrage

   ![POST GraphQL](assets/client-application-integration/post-query-graphql.png)

   Sie sollten eine `POST` zum GraphQL-Endpunkt hinzu. Anzeigen der `Payload`, sehen Sie die vollständige GraphQL-Abfrage, die gesendet wurde. In den nächsten Abschnitten wird die App aktualisiert, um **persistiert** Abfragen.


## Erste Schritte

Im einfachen Tutorial wird eine parametrisierte GraphQl-Abfrage verwendet, um ein einzelnes Inhaltsfragment anzufordern und die Abenteuerdetails zu rendern. Aktualisieren Sie anschließend die `adventureDetailQuery` , um neue Felder einzuschließen und bestehende Abfragen zu verwenden, die im vorherigen Kapitel erstellt wurden.

Es werden fünf Komponenten erstellt:

| React-Komponente | Speicherort |
|-------|------|
| Administrator | `src/components/Administrator.js` |
| Team | `src/components/Team.js` |
| Speicherort | `src/components/Location.js` |
| Instructor | `src/components/Instructors.js` |
| Adresse | `src/components/Address.js` |

## Aktualisieren des useGraphQL-Hooks

Benutzerdefiniert [React Effect Hook](https://reactjs.org/docs/hooks-overview.html#effect-hook) wurde erstellt, die auf Änderungen am `query`und sendet bei Änderung eine HTTP-POST-Anfrage an den AEM GraphQL-Endpunkt und gibt die JSON-Antwort an die App zurück.

Erstellen eines neuen Hooks zur Verwendung **persistiert** Abfragen. Das Programm kann dann HTTP-GET-Anfragen für Abenteuerdetails senden. Die `runPersistedQuery` des [AEM Headless Client SDK](https://github.com/adobe/aem-headless-client-js) wird verwendet, um die Ausführung einer persistenten Abfrage zu vereinfachen.

1. Öffnen Sie die Datei `src/api/useGraphQL.js`
1. Hinzufügen eines neuen Hooks für `useGraphQLPersisted`:

   ```javascript
   /**
   * Custom React Hook to perform a GraphQL query to a persisted query endpoint
   * @param persistedPath - the short path to the persisted query
   * @param fragmentPathParam - optional parameters object that can be passed in for parameterized persistent queries
   */
   export function useGraphQLPersisted(persistedPath, fragmentPathVariable) {
       let [data, setData] = useState(null);
       let [errors, setErrors] = useState(null);
   
       useEffect(() => {
           let queryVariables = {};
   
           // we pass in a primitive fragmentPathVariable (String) and then construct the object {fragmentPath: fragmentPathParam} to pass as query params to the persisted query
           // It is simpler to pass a primitive into a React hooks, as comparing the state of a dependent object can be difficult. see https://reactjs.org/docs/hooks-faq.html#can-i-skip-an-effect-on-updates
           if(fragmentPathVariable) {
               queryVariables = {fragmentPath: fragmentPathVariable};
           }
   
           // execute a persisted query using the given path and pass in variables (if needed)
           sdk.runPersistedQuery(persistedPath, queryVariables)
               .then(({ data, errors }) => {
               if (errors) setErrors(mapErrors(errors));
               if (data) setData(data);
           })
           .catch((error) => {
           setErrors(error);
           });
   }, [persistedPath, fragmentPathVariable]);
   
   return { data, errors }
   }
   ```
1. Speichern Sie die Änderungen in der Datei.

## Komponente &quot;Adventure Details&quot;aktualisieren

Die Datei `src/api/queries.js` enthält die GraphQL-Abfragen, die zum Hochladen der Anwendung verwendet werden `adventureDetailQuery` gibt Details zu einem individuellen Abenteuer mit der standardmäßigen GraphQL-Anfrage der POST zurück. Aktualisieren Sie anschließend die `AdventureDetail` -Komponente zur Verwendung der beibehaltenen `wknd/all-adventure-details` Abfrage.

1. Öffnen Sie `src/screens/AdventureDetail.js`.
1. Führen Sie zunächst einen Kommentar zur folgenden Zeile aus:

   ```javascript
   export default function AdventureDetail() {
   
       ...
   
       //const { data, errors } = useGraphQL(adventureDetailQuery(adventureFragmentPath));
   ```

   Die obige verwendet die standardmäßige GraphQL-POST, um Abenteuerdetails abzurufen, die auf einer `adventureFragmentPath`

1. So verwenden Sie die `useGraphQLPersisted` -Erweiterungspunkt hinzufügen, fügen Sie die folgende Zeile hinzu:

   ```javascript
   export default function AdventureDetail() {
   
      //const { data, errors } = useGraphQL(adventureDetailQuery(adventureFragmentPath));
       const {data, errors} = useGraphQLPersisted("wknd/all-adventure-details", adventureFragmentPath);
   ```

   Pfad beobachten `wknd/all-adventure-details` ist der Pfad zur gespeicherten Abfrage, die im vorherigen Kapitel erstellt wurde.

   >[!CAUTION]
   >
   > Damit die aktualisierte Abfrage funktioniert, muss `wknd/all-adventure-details` muss in der Ziel-AEM-Umgebung beibehalten werden. Überprüfen Sie die Schritte unter [Beständige GraphQL-Abfragen](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md#cache-control-all-adventures) oder installieren Sie die [AEM](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip)

1. Kehren Sie zur App zurück, die im Browser ausgeführt wird, und verwenden Sie die Entwicklertools Ihres Browsers, um die Anforderung zu überprüfen, nachdem Sie zu einer **Adventure-Details** Seite.

   ![Anfrage abrufen](assets/client-application-integration/get-request-persisted-query.png)

   ```
   http://localhost:3000/graphql/execute.json/wknd/all-adventure-details;fragmentPath=/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking
   ```

   Sie sollten jetzt eine `GET` Anfrage, die die persistente Abfrage unter `wknd/all-adventure-details`.

1. Navigieren Sie zu anderen Erlebnisdetails und beobachten Sie, dass dieselbe `GET` -Anfrage erfolgt jedoch mit unterschiedlichen Fragmentpfaden. Die Anwendung sollte weiterhin wie bisher funktionieren.

Siehe `AdventureDetail.js` im [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) für ein vollständiges Beispiel der aktualisierten Komponente.

Erstellen Sie anschließend die **Standort**, **Administrator** und **Instructor** Komponenten zum Rendern von Standortdaten. Die **Adresse** -Komponente wird innerhalb der **Team** -Komponente.

## Entwickeln der Komponente &quot;Position&quot;

1. Im `AdventureDetail.js` -Datei, fügen Sie einen Verweis zum `<Location>` -Komponente, die die Standortdaten aus der `adventure` Datenobjekt:

   ```javascript
   export default function AdventureDetail() {
       ...
   
       return (
           ...
   
           <Location data={adventure.location} />
   ```

1. Überprüfen Sie die Datei unter `src/components/Location.js`. Die `Location` -Komponente rendert die Daten, für die der Kontakt erfüllt werden soll, Kontaktinformationen, Informationen über das Wetter und ein Standortbild aus der **Standort** Inhaltsfragmentmodell. Die Variable `Location` Komponente erwartet eine `address` -Objekt, das übergeben werden soll.
1. Siehe `Location.js` im [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) für ein vollständiges Beispiel der aktualisierten Komponente.

Nachdem die Aktualisierungen vorgenommen wurden, sollte die gerenderte Detailseite wie folgt aussehen:

![location-component](assets/client-application-integration/location-component.png)

## Entwickeln der Team-Komponente

1. Im `AdventureDetail.js` -Datei, fügen Sie einen Verweis zum `<Team>` -Komponente (unter `<Location>` -Komponente) übergeben `instructorTeam` Daten aus `adventure` Datenobjekt:

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam} />
   ```

1. Überprüfen Sie die Datei unter `src/components/Team.js`. Die `Team` -Komponente rendert Daten über das Gründungsdatum, das Bild und die Beschreibung des Teams aus der **Team** Inhaltsfragment.

1. In `Team.js` zur Kenntnis nehmen, dass die `Address` -Komponente.

   ```javascript
   export default function Team({data}) {
       ...
       {teamPath && <Address _path={teamPath}/>}
   ```

   Hier wird ein Pfad zum aktuellen Team an die `Address` -Komponente, die wiederum eine Abfrage ausführt, um die Adresse basierend auf dem Team abzurufen.

1. Siehe `Team.js` im [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) für ein vollständiges Beispiel der Komponente.

Nach der Integration der Abfrage sollte sie wie folgt aussehen:

![team-component](assets/client-application-integration/address-component.png)

## Entwickeln der Komponente Adresse

1. Überprüfen Sie die Datei unter `src/components/Address.js`. Die `Address` Komponenten rendern Adressinformationen wie Straße, Stadt, Bundesland, Postleitzahl und Land aus dem **Adresse** Inhaltsfragment und Telefon und E-Mail aus dem **Kontaktangaben** Fragmentverweis.
1. Die `Address` -Komponente ähnelt dem `AdventureDetails` -Komponente ein, da sie einen persistenten Aufruf zum Abrufen von Daten basierend auf einem Pfad durchführt. Der Unterschied besteht darin, dass er `/wknd/team-location-by-location-path` , um die Anfrage zu stellen.
1. Siehe `Address.js` im [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) für ein vollständiges Beispiel der Komponente.

## Entwickeln der Komponente &quot;Administrator&quot;

1. Im `AdventureDetail.js` -Datei, fügen Sie einen Verweis zum `<Adminstrator>` -Komponente (unter `<Team>` -Komponente) übergeben `administrator` Daten aus `adventure` Datenobjekt:

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam} />
   <Administrator data={adventure.administrator} /> 
   ```

1. Überprüfen Sie die Datei unter `src/components/Administrator.js`. Die `Administrator` Komponente rendert Details wie ihren vollständigen Namen aus der **Administrator** Inhaltsfragment und Rendering von Telefon und E-Mail aus der **Kontaktangaben** Fragmentverweis.
1. Siehe `Administrator.js` in [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) für ein vollständiges Beispiel der Komponente.

Nachdem Sie die Komponente &quot;Administrator&quot;erstellt haben, können Sie die Anwendung rendern. Die Ausgabe sollte mit dem folgenden Bild übereinstimmen:

![administrator-component](assets/client-application-integration/administrator-component.png)

## Entwickeln der Komponente &quot;Instructors&quot;

1. Im `AdventureDetail.js` -Datei, fügen Sie einen Verweis zum `<Instructors>` -Komponente (unter `<Administrator>` -Komponente) übergeben `instructorTeam` Daten aus `adventure` Datenobjekt:

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam}/>
   <Administrator data={adventure.administrator} />             
   <Instructors data={adventure.instructorTeam} />
   ```

1. Überprüfen Sie die Datei unter `src/components/Instructors.js`. Die `Instructors` -Komponente rendert Daten zu den einzelnen Team-Mitgliedern, einschließlich vollständiger Namen, Biografie, Bild, Telefonnummer, Erlebnisebene und Fähigkeiten. Die Komponente iteriert über ein Array, um jedes Element anzuzeigen.
1. Siehe `Instructors.js` in [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) für ein vollständiges Beispiel der Komponente.

Nachdem Sie die Anwendung gerendert haben, sollte die Ausgabe mit dem folgenden Bild übereinstimmen:

![Instructors-component](assets/client-application-integration/instructors-component.png)

## Abgeschlossene WKND-Beispielanwendung

Die fertige App sollte wie folgt aussehen:

![AEM-headless-final-experience](assets/client-application-integration/aem-headless-final-experience.gif)

### Endgültige Clientanwendung

Die endgültige Version des Programms kann heruntergeladen und verwendet werden:
**[aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip)**

## Herzlichen Glückwunsch

Herzlichen Glückwunsch! Sie haben die Integration abgeschlossen und die persistenten Abfragen in die WKND-Beispielanwendung implementiert.
