---
title: Bootstrapping der Remote-SPA für SPA-Editor
description: Erfahren Sie, wie Sie ein Bootstrapping einer Remote-SPA für Kompatibilität mit dem AEM-SPA-Editor durchführen.
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: ht
source-wordcount: '1200'
ht-degree: 100%

---

# Bootstrapping der Remote-SPA für SPA-Editor

Bevor die bearbeitbaren Bereiche zur Remote-SPA hinzugefügt werden können, müssen sie mit dem JavaScript-SDK für den AEM-SPA-Editor und einigen anderen Konfigurationen ein Bootstrapping durchlaufen.

## Installieren von npm-Abhängigkeiten für das AEM SPA Editor JS SDK 

Überprüfen Sie zunächst npm-Abhängigkeiten von AEM für das React-Projekt und installieren Sie sie dann.

+ [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager): stellt die API zum Abrufen von Inhalten aus AEM bereit.
+ [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping): stellt die API bereit, die AEM-Inhalte SPA-Komponenten zuordnet.
+ [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components): stellt eine API zum Erstellen benutzerdefinierter SPA-Komponenten bereit und bietet gängige Implementierungen wie die `AEMPage`-React-Komponente.

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## Überprüfen von SPA-Umgebungsvariablen

Mehrere Umgebungsvariablen müssen der Remote-SPA zur Verfügung gestellt werden, damit sie erkennt, wie mit AEM interagiert werden kann.

1. Öffnen Sie das Remote-SPA-Projekt unter `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` in Ihrer IDE
1. Öffnen Sie die Datei `.env.development`
1. Achten Sie in der Datei besonders auf die Schlüssel und aktualisieren Sie sie nach Bedarf:

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   ![Remote-SPA-Umgebungsvariablen](./assets/spa-bootstrap/env-variables.png)

   *Beachten Sie, dass benutzerdefinierte Umgebungsvariablen in React mit dem Präfix `REACT_APP_` versehen werden müssen.*

   + `REACT_APP_HOST_URI`: das Schema und der Host des AEM-Services, mit denen sich die Remote-SPA verbindet.
      + Dieser Wert ändert sich je nach der AEM-Umgebung (lokal, Entwicklung, Staging oder Produktion) und dem AEM-Service-Typ (Autor vs. Veröffentlichung)
   + `REACT_APP_USE_PROXY`: Dies vermeidet CORS-Probleme während der Entwicklung, indem der React-Entwicklungs-Server AEM-Anfragen wie `/content, /graphql, .model.json` mit dem Modul `http-proxy-middleware` weiterleitet.
   + `REACT_APP_AUTH_METHOD`: Authentifizierungsmethode für von AEM bereitgestellte Anfragen. Optionen sind &#39;service-token&#39;, &#39;dev-token&#39;, &#39;basic&#39; oder leer lassen für einen Anwendungsfall ohne Authentifizierung
      + Erforderlich für die Verwendung mit der AEM-Autoreninstanz
      + Möglicherweise erforderlich für die Verwendung mit AEM Publish (wenn Inhalte geschützt sind)
      + Die Entwicklung mit dem AEM SDK unterstützt lokale Konten über die Standardauthentifizierung. Dies ist die in diesem Tutorial verwendete Methode.
      + Verwenden Sie bei der Integration mit AEM as a Cloud Service [Zugriffstoken](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=de)
   + `REACT_APP_BASIC_AUTH_USER`: Der AEM-__Benutzername__ durch die SPA zur Authentifizierung, während AEM-Inhalte abgerufen werden.
   + `REACT_APP_BASIC_AUTH_PASS`: das AEM-__Kennwort__ durch die SPA zur Authentifizierung, während AEM-Inhalte abgerufen werden.

## Integrieren der ModelManager-API

Wenn npm-Abhängigkeiten für die App verfügbar sind, initialisieren Sie den AEM-`ModelManager` in `index.js` des Projekts, bevor `ReactDOM.render(...)` aufgerufen wird.

Der [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) ist für die Verbindung mit AEM zum Abrufen bearbeitbarer Inhalte verantwortlich.

1. Öffnen Sie das Remote-SPA-Projekt in Ihrer IDE
1. Öffnen Sie die Datei `src/index.js`
1. Fügen Sie „import `ModelManager`“ hinzu und initialisieren Sie dies vor dem Aufruf von `root.render(..)`,

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

Die Datei `src/index.js` sollte wie folgt aussehen:

![src/index.js](./assets/spa-bootstrap/index-js.png)

## Einrichten eines internen SPA-Proxys

Beim Erstellen einer bearbeitbaren SPA ist es am besten, einen [internen Proxy in der SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) einzurichten, der so konfiguriert ist, dass er die passenden Anfragen an AEM weiterleitet. Dies geschieht durch die Verwendung des npm-Moduls [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware), das bereits durch die WKND-GraphQL-App installiert wird.

1. Öffnen Sie das Remote-SPA-Projekt in Ihrer IDE
1. Öffnen Sie die Datei unter `src/proxy/setupProxy.spa-editor.auth.basic.js`
1. Aktualisieren Sie die Datei mit dem folgenden Code:

   ```javascript
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_BASIC_AUTH_USER, REACT_APP_BASIC_AUTH_PASS } = process.env;
   
   /*
       Set up a proxy with AEM for local development
       In a production environment this proxy should be set up at the webserver level or absolute URLs should be used.
   */
   module.exports = function(app) {
   
       /**
       * Filter to check if the request should be re-routed to AEM. The paths to be re-routed at:
       * - Starts with /content (AEM content)
       * - Starts with /graphql (AEM graphQL endpoint)
       * - Ends with .model.json (AEM Content Services)
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns true if the SPA request should be re-routed to AEM
       */
       const toAEM = function(path, req) {
           return path.startsWith('/content') || 
               path.startsWith('/graphql') ||
               path.endsWith('.model.json')
       }
   
       /**
       * Re-writes URLs being proxied to AEM such that they can resolve to real AEM resources
       * - The "root" case of `/.model.json` are rewritten to the SPA's home page in AEM
       * - .model.json requests for /adventure:xxx routes are rewritten to their corresponding adventure page under /content/wknd-app/us/en/home/adventure/ 
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns returns a re-written path, or nothing to use the @param path
       */
       const pathRewriteToAEM = function (path, req) { 
           if (path === '/.model.json') {
               return '/content/wknd-app/us/en/home.model.json';
           } else if (path.startsWith('/adventure/') && path.endsWith('.model.json')) {
               return '/content/wknd-app/us/en/home/adventure/' + path.split('/').pop();
           }    
       }
   
       /**
       * Register the proxy middleware using the toAEM filter and pathRewriteToAEM rewriter 
       */
       app.use(
           createProxyMiddleware(
               toAEM, // Only route the configured requests to AEM
               {
                   target: REACT_APP_HOST_URI,
                   changeOrigin: true,
                   // Pass in credentials when developing against an Author environment
                   auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`,
                   pathRewrite: pathRewriteToAEM // Rewrite SPA paths being sent to AEM
               }
           )
       );
   
       /**
       * Enable CORS on requests from the SPA to AEM
       * 
       * If this rule is not in place, CORS errors will occur when running the SPA on http://localhost:3000
       */
       app.use((req, res, next) => {
           res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);
           next();
       });
   };
   ```

   Die Datei `setupProxy.spa-editor.auth.basic.js` sollte wie folgt aussehen:

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Diese Proxy-Konfiguration hat zwei Hauptaufgaben:

   1. Sie leitet spezifische Anforderungen an die SPA (`http://localhost:3000`) an AEM `http://localhost:4502` weiter.
      + Es werden nur Anforderungen weitergeleitet, deren Pfade mit Mustern übereinstimmen, die angeben, dass sie von AEM bedient werden sollen, wie in `toAEM(path, req)` definiert.
      + SPA-Pfade werden auf die entsprechenden AEM-Seiten umgeschrieben, wie in `pathRewriteToAEM(path, req)` definiert
   1. Allen Anfragen werden CORS-Header hinzugefügt, um den Zugriff auf AEM-Inhalte zu ermöglichen, wie in `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);` definiert
      + Wenn dies nicht hinzugefügt wird, treten CORS-Fehler beim Laden von AEM-Inhalten in der SPA auf.

1. Öffnen Sie die Datei `src/setupProxy.js`
1. Überprüfen Sie die Zeile, die auf die Proxy-Konfigurationsdatei `setupProxy.spa-editor.auth.basic` hinweist:

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

Beachten Sie: Alle Änderungen an `src/setupProxy.js` oder den darin referenzierten Dateien erfordern einen Neustart der SPA.

## Statische SPA-Ressource

Statische SPA-Ressourcen wie das WKND-Logo und die Ladegrafiken benötigen eine Aktualisierung ihrer src-URLs, damit sie vom Remote-SPA-Host geladen werden. Wenn diese URLs relativ bleiben und die SPA zum Erstellen im SPA-Editor geladen wird, verwenden diese URLs standardmäßig einen AEM-Host anstelle der SPA, was zu 404-Anfragen führt, wie in der Abbildung unten dargestellt.

![Beschädigte statische Ressourcen](./assets/spa-bootstrap/broken-static-resource.png)

Um dieses Problem zu beheben, stellen Sie sicher, dass eine statische Ressource, die von der Remote-SPA gehostet wird, absolute Pfade verwendet, die die Herkunft der Remote-SPA enthalten.

1. Öffnen Sie das SPA-Projekt in Ihrer IDE.
1. Öffnen Sie Ihre Datei der SPA-Umgebungsvariablen `src/.env.development` und fügen Sie eine Variable für den öffentlichen URI der SPA hinzu:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Bei der Bereitstellung in AEM as a Cloud Service müssen Sie für die entsprechenden `.env`-Dateien genauso vorgehen._

1. Öffnen Sie die Datei `src/App.js`
1. Importieren des öffentlichen SPA-URI aus den SPA-Umgebungsvariablen

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. Stellen Sie dem WKND-Logo `<img src=.../>` das Präfix `REACT_APP_PUBLIC_URI` voran, um eine Auflösung gegen die SPA zu erzwingen.

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. Tun Sie dasselbe für das Ladebild in `src/components/Loading.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. Und für die __zwei Instanzen__ der Schaltfläche „Zurück“ in `src/components/AdventureDetails.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

Die Dateien `App.js`, `Loading.js` und `AdventureDetails.js` sollten wie folgt aussehen:

![Statische Ressourcen](./assets/spa-bootstrap/static-resources.png)

## Responsives Raster von AEM

Um den Layout-Modus des SPA-Editors für bearbeitbare Bereiche in der SPA zu unterstützen, müssen wir das AEM-CSS für responsive Raster in die SPA integrieren. Machen Sie sich keine Sorgen – dieses Rastersystem gilt nur für die bearbeitbaren Container, und Sie können ein Rastersystem Ihrer Wahl verwenden, um das Layout der restlichen SPA zu steuern.

Fügen Sie die SCSS-Dateien des responsiven Rasters von AEM zur SPA hinzu.

1. Öffnen Sie das SPA-Projekt in Ihrer IDE.
1. Laden Sie die beiden folgenden Dateien herunter und kopieren Sie sie in `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + Der SCSS-Generator responsiver Raster von AEM
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + Ruft `_grid.scss` durch Verwendung der SPA-spezifischen Haltepunkte (Desktop und Mobile) und Spalten (12) auf.
1. Öffnen Sie `src/App.scss` und importieren Sie `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

Die Dateien `_grid.scss` und `_grid-init.scss` sollten wie folgt aussehen:

![AEM-SCSS für responsive Raster](./assets/spa-bootstrap/aem-responsive-grid.png)

Jetzt enthält die SPA das erforderliche CSS, um den Layout-Modus von AEM für Komponenten zu unterstützen, die einem AEM-Container hinzugefügt wurden.

## Dienstprogrammklassen

Kopieren Sie die folgenden Dienstprogrammklassen in Ihr React-App-Projekt.

+ [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) nach `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
+ [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) nach `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
+ [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) nach `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
+ [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) nach `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![Remote-SPA-Dienstprogrammklassen](./assets/spa-bootstrap/utility-classes.png)

## Starten Sie die SPA

Nachdem ein Bootstrapping der SPA für die Integration mit AEM durchgeführt wurde, lassen Sie uns nun die SPA ausführen und schauen, wie sie aussieht!

1. Navigieren Sie in der Befehlszeile zum Stammverzeichnis des SPA-Projekts.
1. Starten Sie die SPA mit den normalen Befehlen (falls noch nicht geschehen)

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. Durchsuchen Sie die SPA auf [http://localhost:3000](http://localhost:3000). Alles sollte gut aussehen!

![SPA läuft auf http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## Öffnen Sie die SPA im AEM-SPA-Editor.

Während die SPA auf [http://localhost:3000](http://localhost:3000) läuft, öffnen wir sie mit dem AEM-SPA-Editor. In der SPA ist noch nichts bearbeitbar, dies validiert nur die SPA in AEM.

1. Melden Sie sich bei AEM Author an
1. Navigieren Sie zu __Sites > WKND App > us > en__
1. Wählen Sie die __WKND-App-Startseite__ und tippen Sie auf __Bearbeiten__, woraufhin die SPA angezeigt wird.

   ![Bearbeiten der WKND-App-Startseite](./assets/spa-bootstrap/edit-home.png)

1. Wechseln Sie zur __Vorschau__ über den Modusschalter oben rechts
1. Klicken Sie sich in der SPA durch

   ![SPA läuft auf http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)

## Herzlichen Glückwunsch!

Sie haben ein Bootstrapping der Remote-SPA vorgenommen, damit sie mit dem AEM-SPA-Editor kompatibel ist. Sie wissen jetzt, wie man Folgendes tut:

+ Hinzufügen der npm-Abhängigkeiten des AEM SPA Editor JS SDK zum SPA-Projekt
+ Konfigurieren Ihrer SPA-Umgebungsvariablen
+ Integrieren der ModelManager-API in die SPA
+ Einrichten eines internen Proxys für die SPA, damit die passenden Inhaltsanfragen an AEM weitergeleitet werden
+ Beheben von Problemen mit statischen SPA-Ressourcen, die im Kontext des SPA-Editors aufgelöst werden
+ Hinzufügen von CSS für das responsive Raster von AEM, um Layout-Änderungen in bearbeitbaren Containern von AEM zu unterstützen

## Nächste Schritte

Nachdem wir nun eine grundsätzliche Kompatibilität mit dem AEM-SPA-Editor erreicht haben, können wir damit beginnen, bearbeitbare Bereiche einzuführen. Wir schauen uns zunächst an, wie wir eine [feste bearbeitbare Komponente](./spa-fixed-component.md) in der SPA platzieren.
