---
title: AEM Headless-APIs und React - Erstes Tutorial AEM Headless
description: Erfahren Sie, wie Sie das Abrufen von Inhaltsfragmentdaten aus AEM GraphQL-APIs und deren Anzeige in der React-App abdecken.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 2b726473-5a32-4046-bce8-6da3c57a1b60
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 1%

---

# AEM Headless-APIs und React

Willkommen in diesem Tutorial-Kapitel, in dem wir die Konfiguration einer React-App für die Verbindung mit Adobe Experience Manager (AEM) Headless-APIs mithilfe des AEM Headless-SDK untersuchen. Wir behandeln das Abrufen von Inhaltsfragmentdaten aus AEM GraphQL-APIs und die Anzeige dieser Daten in der React-App.

AEM Headless-APIs ermöglichen den Zugriff auf AEM Inhalt von jeder Client-App aus. Wir führen Sie durch die Konfiguration Ihrer React-App für die Verbindung mit AEM Headless-APIs mithilfe des AEM Headless-SDK. Durch dieses Setup wird ein wiederverwendbarer Kommunikationskanal zwischen Ihrer React-App und AEM eingerichtet.

Als Nächstes verwenden wir das AEM Headless-SDK, um Inhaltsfragmentdaten aus AEM GraphQL-APIs abzurufen. Inhaltsfragmente in AEM bieten strukturiertes Content Management. Mithilfe des AEM Headless SDK können Sie Inhaltsfragmentdaten mithilfe von GraphQL einfach abfragen und abrufen.

Sobald wir über die Inhaltsfragmentdaten verfügen, werden wir sie in Ihre React-App integrieren. Sie werden lernen, die Daten in ansprechender Weise zu formatieren und anzuzeigen. Wir werden Best Practices für die Handhabung und Darstellung von Inhaltsfragmentdaten in React-Komponenten behandeln und so eine nahtlose Integration mit der Benutzeroberfläche Ihrer App sicherstellen.

Während des Tutorials werden wir Erklärungen, Codebeispiele und praktische Tipps bereitstellen. Am Ende können Sie Ihre React-App so konfigurieren, dass eine Verbindung zu AEM Headless-APIs hergestellt, Inhaltsfragmentdaten mit dem AEM Headless-SDK abgerufen und nahtlos in Ihrer React-App angezeigt werden. Fangen wir an!


## React-App klonen

1. Klonen Sie die App von [Github](https://github.com/lamontacrook/headless-first/tree/main) durch Ausführen des folgenden Befehls in der Befehlszeile.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Änderungen in der `headless-first` und installieren Sie die Abhängigkeiten.

   ```
   $ cd headless-first
   $ npm ci
   ```

## Konfigurieren der React-App

1. Erstellen Sie eine Datei mit dem Namen `.env` am Stamm des Projekts. In `.env` Legen Sie die folgenden Werte fest:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Sie können ein Entwicklungstoken in Cloud Manager abrufen. Anmelden bei [Adobe Cloud Manager](https://experience.adobe.com/). Klicks __Experience Manager > Cloud Manager__. Wählen Sie das entsprechende Programm aus und klicken Sie dann auf die Auslassungspunkte neben der Umgebung.

   ![AEM Developer Console](./assets/2/developer-console.png)

   1. Klicken Sie in der __Integrationen__ tab
   1. Klicks __Registerkarte &quot;Lokales Token&quot;und lokales Entwicklungstoken abrufen__ button
   1. Kopieren Sie das Zugriffstoken, das nach dem offenen Anführungszeichen beginnt, bis vor dem schließenden Anführungszeichen.
   1. Fügen Sie das kopierte Token als Wert für `REACT_APP_TOKEN` im `.env` -Datei.
   1. Erstellen wir nun die App, indem wir `npm ci` in der Befehlszeile.
   1. Starten Sie jetzt die React-App und führen Sie `npm run start` in der Befehlszeile.
   1. In [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) eine Datei mit dem Namen `context.js`  enthält den Code zum Festlegen der Werte in der `.env` -Datei in den Kontext der App.

## Ausführen der React-App

1. Starten Sie die React-App, indem Sie `npm run start` in der Befehlszeile.

   ```
   $ npm run start
   ```

   Die React-App startet und öffnet ein Browserfenster, in dem `http://localhost:3000`. Änderungen an der React-App werden automatisch im Browser neu geladen.

## Verbindung zu AEM Headless-APIs

1. Um die React-App mit AEM as a Cloud Service zu verbinden, fügen wir ein paar Dinge hinzu `App.js`. Im `React` importieren, hinzufügen `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Import `AppContext` aus dem `context.js` -Datei.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Definieren Sie jetzt im App-Code eine Kontextvariable.

   ```javascript
   const context = useContext(AppContext);
   ```

   Und schließen Sie schließlich den Rückgabe-Code in `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   Als Referenz dient die `App.js` sollte nun so sein.

   ```javascript
   import React, {useContext} from 'react';
   import './App.css';
   import { BrowserRouter, Routes, Route } from 'react-router-dom';
   import Home from './screens/home/home';
   import { AppContext } from './utils/context';
   
   const App = () => {
   const context = useContext(AppContext);
   return (
       <div className='App'>
       <AppContext.Provider value={context}>
           <BrowserRouter>
           <Routes>
               <Route exact={true} path={'/'} element={<Home />} />
           </Routes>
           </BrowserRouter>
       </AppContext.Provider>
       </div>
   );
   };
   
   export default App;
   ```

1. Importieren Sie `AEMHeadless` SDK. Dieses SDK ist eine Hilfsbibliothek, die von der App zur Interaktion mit AEM Headless-APIs verwendet wird.

   Fügen Sie diese Importanweisung zum `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Fügen Sie Folgendes hinzu: `{ useContext, useEffect, useState }` der` React` Importanweisung.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importieren Sie `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   Innerhalb des `Home` -Komponente, rufen Sie die `context` aus der `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Initialisieren des AEM Headless SDK in einem  `useEffect()`, da das AEM Headless-SDK sich ändern muss, wenn die Variable  `context` ändern.

   ```javascript
   useEffect(() => {
   const sdk = new AEMHeadless({
       serviceURL: context.url,
       endpoint: context.endpoint,
       auth: context.token
   });
   }, [context]);  
   ```

   >[!NOTE]
   >
   > Es gibt eine `context.js` Datei unter `/utils` , das Elemente aus dem `.env` -Datei. Als Referenz dient die `context.url` ist die URL der AEM as a Cloud Service Umgebung. Die `context.endpoint` ist der vollständige Pfad zum in der vorherigen Lektion erstellten Endpunkt. Schließlich `context.token` ist das Entwickler-Token.


1. Erstellen Sie einen React-Status, der die Inhalte aus dem AEM Headless SDK verfügbar macht.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Verbinden Sie die App mit AEM. Verwenden Sie die in der vorherigen Lektion erstellte persistente Abfrage. Fügen wir den folgenden Code innerhalb der `useEffect` nachdem das AEM Headless-SDK initialisiert wurde. Stellen Sie die `useEffect` abhängig von der  `context` wie unten dargestellt.


   ```javascript
   useEffect(() => {
   ...
   sdk.runPersistedQuery('<name of the endpoint>/<name of the persisted query>', { path: `/content/dam/${context.project}/<name of the teaser fragment>` })
       .then(({ data }) => {
       if (data) {
           setContent(data);
       }
       })
       .catch((error) => {
       console.log(`Error with pure-headless/teaser. ${error.message}`);
       });
   }, [context]);
   ```

1. Öffnen Sie die Netzwerkansicht der Entwicklertools, um die GraphQL-Anforderung zu überprüfen.

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Chrome Dev Tools](./assets/2/dev-tools.png)

   Das AEM Headless-SDK kodiert die Anforderung für GraphQL und fügt die bereitgestellten Parameter hinzu. Sie können die Anfrage im Browser öffnen.

   >[!NOTE]
   >
   > Da die Anfrage in die Autorenumgebung geleitet wird, müssen Sie in einer anderen Registerkarte desselben Browsers in der Umgebung angemeldet sein.


## Inhaltsfragmentinhalt rendern

1. Anzeigen der Inhaltsfragmente in der App. Rückgabe einer `<div>` mit dem Titel des Teasers.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Das Titelfeld des Teasers sollte auf dem Bildschirm angezeigt werden.

1. Der letzte Schritt besteht darin, den Teaser zur Seite hinzuzufügen. Eine React-Teaser-Komponente ist im Paket enthalten. Nehmen wir zunächst den Import auf. Oben im `home.js` -Datei, fügen Sie die Zeile hinzu:

   `import Teaser from '../../components/teaser/teaser';`

   Aktualisieren Sie die return-Anweisung:

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   Jetzt sollte der Teaser mit dem Inhalt im Fragment angezeigt werden.


## Nächste Schritte

Herzlichen Glückwunsch! Sie haben die React-App erfolgreich aktualisiert, um sie mit AEM Headless-APIs mit dem AEM Headless-SDK zu integrieren!

Als Nächstes erstellen wir eine komplexere Bildlisten-Komponente, die referenzierte Inhaltsfragmente dynamisch aus AEM rendert.

[Nächstes Kapitel: Erstellen einer Bildlisten-Komponente](./3-complex-components.md)
