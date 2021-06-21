---
title: Bootstrap der Remote-SPA für SPA Editor
description: Erfahren Sie, wie Sie ein Remote-SPA für AEM Editor-Kompatibilität mit SPA Bootstrapping durchführen.
topic: Headless, SPA, Entwicklung
feature: SPA Editor, Kernkomponenten, APIs, Entwicklung
role: Developer, Architect
level: Beginner
kt: 7633
thumbnail: kt-7633.jpeg
source-git-commit: 76b10941ca8aeb5aa15ca39d354d9f7e7fb24522
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 2%

---


# Bootstrap der Remote-SPA für SPA Editor

Bevor die bearbeitbaren Bereiche zur Remote-SPA hinzugefügt werden können, müssen sie mit dem JavaScript-SDK für den AEM SPA Editor und einigen anderen Konfigurationen per Bootstrapping versehen werden.


## WKND-App-Quelle herunterladen

Sofern noch nicht geschehen, laden Sie den Quellcode der WKND-App von Github.com herunter und wechseln Sie die Verzweigung mit den Änderungen an der in diesem Tutorial durchgeführten SPA.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Hinzufügen AEM Editor-JS-SDK-NPM-Abhängigkeiten

Fügen Sie zunächst AEM npm-Abhängigkeiten zum React-Projekt hinzu.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install --save \
    @adobe/aem-spa-page-model-manager \
    @adobe/aem-spa-component-mapping \
    @adobe/aem-react-editable-components \
    @adobe/aem-core-components-react-base \
    @adobe/aem-core-components-react-spa
```

+ `@adobe/aem-spa-page-model-manager` stellt die API zum Abrufen von Inhalten aus AEM bereit.
+ `@adobe/aem-spa-component-mapping` stellt die API bereit, die AEM Inhalt SPA Komponenten zuordnet.
+ ` @adobe/aem-react-editable-components` stellt eine API zum Erstellen benutzerdefinierter SPA-Komponenten bereit und bietet häufig verwendete Implementierungen wie die  `AEMPage` React-Komponente.
+ `@adobe/aem-core-components-react-base` bietet eine Suite einsatzbereiter React-Komponenten, die nahtlos in die AEM WCM-Kernkomponenten integriert werden und SPA Editor-agnostisch sind. Dazu gehören vor allem Inhaltskomponenten wie:
   + Titel
   + Text
   + Breadcrumb
   + Und so weiter.
+ `@adobe/aem-core-components-react-spa` bietet eine Suite einsatzbereiter React-Komponenten, die nahtlos in die AEM WCM-Kernkomponenten integriert werden können, aber einen SPA-Editor erfordern. Diese enthalten hauptsächlich Komponenten, die Inhaltskomponenten von `@adobe/aem-core-components-react-base` enthalten, z. B.:
   + Container
   + Karussell
   + usw.

## SPA Umgebungsvariablen überprüfen

Mehrere Umgebungsvariablen müssen dem Remote-SPA zur Verfügung gestellt werden, damit er weiß, wie mit AEM interagiert werden kann.

1. Öffnen Sie das Remote SPA-Projekt unter `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` in Ihrer IDE.
1. Öffnen Sie die Datei `.env.development`
1. Fügen Sie die Datei hinzu, wobei den Schlüsseln besondere Aufmerksamkeit gilt:

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   ![Remote-SPA Umgebungsvariablen](./assets/spa-bootstrap/env-variables.png)

   *Beachten Sie, dass benutzerdefinierte Umgebungsvariablen in React mit dem Präfix  `REACT_APP_`versehen werden müssen.*

   + `REACT_APP_AEM_URI`: das Schema und den Host des AEM Service, mit dem die Remote-SPA verbindet.
      + Dieser Wert ändert sich je nachdem, ob die AEM Umgebung (lokal, Entwicklung, Staging oder Produktion) und der AEM Service-Typ (Autor vs. Veröffentlichung)
   + `REACT_APP_AEM_AUTH`: die vom SPA verwendeten Anmeldeinformationen, um Inhalte zu AEM und abzurufen.
      + Erforderlich für die Verwendung mit AEM Author
      + Möglicherweise für die Verwendung mit AEM Publish erforderlich (wenn Inhalt geschützt ist)
      + Die Entwicklung mit dem AEM SDK unterstützt lokale Konten über die einfache Authentifizierung. Dies ist die in diesem Tutorial verwendete Methode.
      + Verwenden Sie bei der Integration mit AEM as a Cloud Service [Zugriffstoken](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html).

## Integration der ModelManager-API

Wenn die AEM npm-Abhängigkeiten für die App verfügbar sind, initialisieren Sie AEM `ModelManager` in `index.js` des Projekts, bevor `ReactDOM.render(...)` aufgerufen wird.

Der [ModelManager](https://www.npmjs.com/package/@adobe/aem-spa-page-model-manager) ist für die Verbindung mit AEM zum Abrufen bearbeitbarer Inhalte verantwortlich.

1. Öffnen Sie das Remote SPA-Projekt in Ihrer IDE.
1. Öffnen Sie die Datei `src/index.js`
1. Fügen Sie den Import `ModelManager` hinzu und initialisieren Sie ihn vor dem `ReactDOM.render(..)` -Aufruf.

   ```
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking ReactDOM.render(...).
   ModelManager.initializeAsync();
   
   ReactDOM.render(...);
   ```

Die Datei `src/index.js` sollte wie folgt aussehen:

![src/index.js](./assets/spa-bootstrap/index-js.png)

## Internen SPA Proxy einrichten

Beim Abrufen bearbeitbarer Inhalte aus AEM in der SPA ist es am besten, einen [internen Proxy in der SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) einzurichten, der so konfiguriert ist, dass die entsprechenden Anforderungen an AEM weitergeleitet werden. Dies geschieht mithilfe des npm-Moduls [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware), das bereits von der WKND GraphQL App installiert wird.

1. Öffnen Sie das Remote SPA-Projekt in Ihrer IDE.
1. Erstellen Sie eine Datei unter `src/proxy/setupProxy.spa-editor.auth.basic.js`.
1. Fügen Sie der Datei den folgenden Code hinzu:

   ```
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_AUTHORIZATION } = process.env;
   
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
               path.startsWith('/graphq') ||
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
           } else if (path.startsWith('/adventure:') && path.endsWith('.model.json')) {
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
                   auth: REACT_APP_AUTHORIZATION,
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

   1. Proxyspezifische Anforderungen an die SPA, `http://localhost:3000` bis AEM `http://localhost:4502`
      + Es werden nur Anforderungen proximiert, deren Pfade mit Mustern übereinstimmen, die angeben, dass sie von AEM bedient werden sollen, wie in `toAEM(path, req)` definiert.
      + SPA werden Pfade zu den entsprechenden AEM Seiten neu geschrieben, wie in `pathRewriteToAEM(path, req)` definiert
   1. Es werden CORS-Header zu allen Anforderungen hinzugefügt, um den Zugriff auf AEM Inhalt zu ermöglichen, wie durch `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);` definiert
      + Wenn dies nicht hinzugefügt wird, treten CORS-Fehler beim Laden AEM Inhalts in der SPA auf.

1. Öffnen Sie die Datei `src/setupProxy.js`
1. Kommentarzeichen `const proxy = require('./proxy/setupProxy.auth.basic')`
1. Fügen Sie eine Zeile hinzu, die auf die neue Proxy-Konfigurationsdatei verweist:

   ```
   // Proxy configuration for SPA Editor (and GraphQL) using Basic Auth
   const proxy = require('./proxy/setupProxy.spa-editor.auth.basic')
   ```

   Die Datei `setupProxy.js` sollte wie folgt aussehen:

   ![src/setupProxy.js](./assets/spa-bootstrap/setup-proxy-js.png)

Beachten Sie, dass Änderungen an den `src/setupProxy.js` oder den referenzierten Dateien einen Neustart des SPA erfordern.

## Statische SPA

Statische SPA Ressourcen wie das WKND-Logo und die Grafiken zum Laden müssen ihre src-URLs aktualisieren, damit sie vom Remote-SPA-Host geladen werden. Wenn diese URLs relativ bleiben und der SPA zum Authoring im SPA Editor geladen wird, verwenden sie standardmäßig AEM Host anstelle des SPA, was zu 404-Anfragen führt, wie in der Abbildung unten dargestellt.

![Beschädigte statische Ressourcen](./assets/spa-bootstrap/broken-static-resource.png)

Um dieses Problem zu beheben, stellen Sie sicher, dass eine statische Ressource, die von der Remote-SPA gehostet wird, absolute Pfade verwendet, die die Remote-SPA-Herkunft enthalten.

1. Öffnen Sie das SPA in Ihrer IDE.
1. Öffnen Sie die Datei mit den SPA Umgebungsvariablen `src/.env.development` und fügen Sie eine Variable für den öffentlichen SPA URI hinzu:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Bei der Bereitstellung in AEM als Cloud Service müssen Sie dasselbe für die entsprechenden  `.env` Dateien tun._

1. Öffnen Sie die Datei `src/App.js`
1. Importieren Sie den SPA öffentlichen URI aus den SPA Umgebungsvariablen

   ```
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. Setzen Sie dem WKND-Logo `<img src=.../>` das Präfix `REACT_APP_PUBLIC_URI` voran, um die Auflösung gegen die SPA zu erzwingen.

   ```
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. Dasselbe gilt für das Laden von Bildern in `src/components/Loading.js`

   ```
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. . und für die __zwei Instanzen__ der Schaltfläche &quot;Zurück&quot;in `src/components/AdventureDetails.js`

   ```
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

## AEM responsives Raster

Um den Layout-Modus SPA Editors für bearbeitbare Bereiche im SPA zu unterstützen, müssen wir AEM CSS für responsives Raster in die SPA integrieren. Machen Sie sich keine Gedanken - dieses Rastersystem wird nur zu den bearbeitbaren Containern, und Sie können Ihr Rastersystem Ihrer Wahl verwenden, um das Layout des restlichen SPA zu steuern.

Fügen Sie die SCSS-Dateien AEM responsiven Rasters zum SPA hinzu.

1. Öffnen Sie das SPA in Ihrer IDE.
1. Laden Sie die beiden folgenden Dateien herunter und kopieren Sie sie in `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + Der SCSS-Generator AEM responsiven Rasters
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + Ruft `_grid.scss` mithilfe der SPA spezifischen Haltepunkte (Desktop und Mobilgerät) und Spalten (12) auf.
1. Öffnen Sie `src/App.scss` und importieren Sie `./styles/grid-init.scss`

   ```
   ...
   @import './styles/grid-init';
   ...
   ```

Die Dateien `_grid.scss` und `_grid-init.scss` sollten wie folgt aussehen:

![AEM Responsives Raster-SCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

Jetzt enthält der SPA das CSS, das erforderlich ist, um AEM Layout-Modus für Komponenten zu unterstützen, die einem AEM Container hinzugefügt werden.

## Starten Sie die SPA

Nachdem der SPA für die Integration mit AEM bootstrapping durchgeführt wurde, lassen Sie uns nun den SPA ausführen und sehen, wie er aussieht!

1. Navigieren Sie in der Befehlszeile zum Stammverzeichnis des SPA-Projekts.
1. Starten Sie den SPA mit den normalen Befehlen (führen Sie `npm install` aus, falls Sie noch nicht über verfügen).

   ```
   $ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
   $ npm install 
   $ npm run start
   ```

1. Durchsuchen Sie die SPA unter [http://localhost:3000](http://localhost:3000). Alles sollte gut aussehen!

![SPA läuft auf http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## Öffnen Sie die SPA in AEM SPA Editor.

Wenn die SPA auf [http://localhost:3000](http://localhost:3000) ausgeführt wird, öffnen wir sie mit AEM SPA Editor. In der SPA ist noch nichts bearbeitbar, dies validiert nur die SPA in AEM.

1. Bei der AEM-Autoreninstanz anmelden
1. Navigieren Sie zu __Sites > WKND-App > us > en__
1. Wählen Sie die __WKND-App-Startseite__ aus und tippen Sie auf __Bearbeiten__. Daraufhin wird der SPA angezeigt.

   ![WKND-App-Homepage bearbeiten](./assets/spa-bootstrap/edit-home.png)

1. Wechseln Sie mithilfe des Modusschalters oben rechts zu __Vorschau__ .
1. Klicken Sie um die SPA

   ![SPA läuft auf http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)

## Herzlichen Glückwunsch!

Sie haben das Bootstrapping der Remote SPA vorgenommen, um AEM Editor-kompatibel zu sein SPA! Sie wissen jetzt, wie:

+ Fügen Sie dem SPA Projekt die NPM-Abhängigkeiten des AEM SPA Editor JS SDK hinzu.
+ SPA Umgebungsvariablen konfigurieren
+ Integrieren der ModelManager-API in die SPA
+ Richten Sie einen internen Proxy für den SPA ein, damit die entsprechenden Inhaltsanforderungen an AEM weitergeleitet werden.
+ Beheben von Problemen mit statischen SPA Ressourcen, die im Kontext des SPA-Editors aufgelöst werden
+ Fügen Sie AEM CSS für responsives Raster hinzu, um das Layout AEM bearbeitbaren Containern zu unterstützen

## Nächste Schritte

Nachdem wir nun eine Grundlinie der Kompatibilität mit AEM Editor erreicht haben, können wir damit beginnen, bearbeitbare Bereiche einzuführen. Wir werden uns zunächst ansehen, wie eine [feste bearbeitbare Komponente](./spa-fixed-component.md) in die SPA platziert wird.
