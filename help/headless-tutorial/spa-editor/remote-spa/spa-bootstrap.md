---
title: Bootstrap der Remote-SPA für SPA Editor
description: Erfahren Sie, wie Sie ein Remote-SPA für AEM Editor-Kompatibilität mit SPA Bootstrapping durchführen.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7633
thumbnail: kt-7633.jpeg
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 3%

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

## Überprüfen der NPM-Abhängigkeiten AEM Editor SPA JS SDK

Überprüfen Sie zunächst AEM NPM-Abhängigkeiten zum React-Projekt.

+ [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) : stellt die API zum Abrufen von Inhalten aus AEM bereit.
+ [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) : stellt die API bereit, die AEM Inhalt SPA Komponenten zuordnet.
+ [`@adobe/aem-react-editable-components`](https://github.com/adobe/aem-react-editable-components) : stellt eine API zum Erstellen benutzerdefinierter SPA-Komponenten bereit und bietet gängige Implementierungen wie die `AEMPage` React-Komponente.
+ [`@adobe/aem-core-components-react-base`](https://github.com/adobe/aem-react-core-wcm-components-base) : bietet eine Suite einsatzbereiter React-Komponenten, die nahtlos in die AEM WCM-Kernkomponenten integriert werden und SPA Editor-agnostisch sind. Dazu gehören vor allem Inhaltskomponenten wie:
   + Titel
   + Text
   + Breadcrumb
   + Und so weiter.
+ [`@adobe/aem-core-components-react-spa`](https://github.com/adobe/aem-react-core-wcm-components-spa) : bietet eine Suite einsatzbereiter React-Komponenten, die nahtlos in die AEM WCM-Kernkomponenten integriert werden können, aber einen SPA-Editor erfordern. Diese enthalten hauptsächlich Komponenten, die Inhaltskomponenten aus `@adobe/aem-core-components-react-base`, z. B.:
   + Container
   + Karussell
   + usw.

## SPA Umgebungsvariablen überprüfen

Mehrere Umgebungsvariablen müssen dem Remote-SPA zur Verfügung gestellt werden, damit er weiß, wie mit AEM interagiert werden kann.

1. Öffnen Sie das Remote SPA-Projekt unter `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` in Ihrer IDE
1. Öffnen Sie die Datei `.env.development`
1. Achten Sie in der Datei besonders auf die Schlüssel:

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   ![Remote-SPA Umgebungsvariablen](./assets/spa-bootstrap/env-variables.png)

   *Beachten Sie, dass benutzerdefinierte Umgebungsvariablen in React mit dem Präfix `REACT_APP_`.*

   + `REACT_APP_HOST_URI`: das Schema und den Host des AEM Service, mit dem die Remote-SPA verbindet.
      + Dieser Wert ändert sich je nachdem, ob die AEM Umgebung (lokal, Entwicklung, Staging oder Produktion) und der AEM Service-Typ (Autor vs. Veröffentlichung)
   + `REACT_APP_USE_PROXY`: Dies vermeidet CORS-Probleme während der Entwicklung, indem der React Development Server AEM Proxy-Anfragen wie `/content, /graphql, .model.json` using `http-proxy-middleware` -Modul.
   + `REACT_APP_AUTH_METHOD`: Authentifizierungsmethode für AEM bereitgestellten Anfragen, Optionen sind &quot;service-token&quot;, &quot;dev-token&quot;, &quot;basic&quot; oder leer lassen für Anwendungsfall ohne Authentifizierung
      + Erforderlich für die Verwendung mit AEM Author
      + Möglicherweise für die Verwendung mit AEM Publish erforderlich (wenn Inhalt geschützt ist)
      + Die Entwicklung mit dem AEM SDK unterstützt lokale Konten über die einfache Authentifizierung. Dies ist die in diesem Tutorial verwendete Methode.
      + Verwenden Sie bei der Integration mit AEM as a Cloud Service [Zugriffstoken](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
   + `REACT_APP_BASIC_AUTH_USER`: AEM __Benutzername__ durch die SPA zu authentifizieren, während AEM Inhalt abgerufen wird.
   + `REACT_APP_BASIC_AUTH_PASS`: AEM __password__ durch die SPA zu authentifizieren, während AEM Inhalt abgerufen wird.

## Integration der ModelManager-API

Initialisieren Sie AEM mit den npm-Abhängigkeiten, die für die App verfügbar sind, AEM `ModelManager` im Projekt `index.js` before `ReactDOM.render(...)` aufgerufen wird.

Die [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) ist für die Verbindung mit AEM zum Abrufen bearbeitbarer Inhalte verantwortlich.

1. Öffnen Sie das Remote SPA-Projekt in Ihrer IDE.
1. Öffnen Sie die Datei `src/index.js`
1. Import hinzufügen `ModelManager` und initialisieren Sie sie vor dem `ReactDOM.render(..)` Aufruf,

   ```
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking ReactDOM.render(...).
   ModelManager.initializeAsync();
   
   ReactDOM.render(...);
   ```

Die `src/index.js` sollte wie folgt aussehen:

![src/index.js](./assets/spa-bootstrap/index-js.png)

## Internen SPA Proxy einrichten

Wenn Sie bearbeitbaren Inhalt aus AEM in der SPA beziehen, sollten Sie am besten eine [interner Proxy im SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually), der so konfiguriert ist, dass die entsprechenden Anfragen an AEM weitergeleitet werden. Dies geschieht durch Verwendung von [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm-Modul, das bereits von der WKND GraphQL-Basisanwendung installiert wird.

1. Öffnen Sie das Remote SPA-Projekt in Ihrer IDE.
1. Öffnen Sie die Datei  unter `src/proxy/setupProxy.spa-editor.auth.basic.js`
1. Überprüfen Sie den folgenden Code:

   ```
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

   Die `setupProxy.spa-editor.auth.basic.js` sollte wie folgt aussehen:

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Diese Proxy-Konfiguration hat zwei Hauptaufgaben:

   1. Proxyspezifische Anforderungen an die SPA, `http://localhost:3000` AEM `http://localhost:4502`
      + Es werden nur Anforderungen proximiert, deren Pfade mit Mustern übereinstimmen, die angeben, dass sie von AEM bedient werden sollen, wie definiert in `toAEM(path, req)`.
      + SPA werden Pfade zu den entsprechenden AEM Seiten neu geschrieben, wie in `pathRewriteToAEM(path, req)`
   1. Dadurch werden allen Anfragen CORS-Header hinzugefügt, um den Zugriff auf AEM Inhalt zu ermöglichen, wie in `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + Wenn dies nicht hinzugefügt wird, treten CORS-Fehler beim Laden AEM Inhalts in der SPA auf.

1. Öffnen Sie die Datei `src/setupProxy.js`
1. Überprüfen Sie die Zeile, die auf die `setupProxy.spa-editor.auth.basic` Proxy-Konfigurationsdatei:

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

   Die `setupProxy.js` sollte wie folgt aussehen:

   ![src/setupProxy.js](./assets/spa-bootstrap/setup-proxy-js.png)

Beachten Sie alle Änderungen am `src/setupProxy.js` oder die referenzierten Dateien einen Neustart des SPA erfordern.

## Statische SPA

Statische SPA Ressourcen wie das WKND-Logo und die Grafiken zum Laden müssen ihre src-URLs aktualisieren, damit sie vom Remote-SPA-Host geladen werden. Wenn diese URLs relativ bleiben und der SPA zum Authoring im SPA Editor geladen wird, verwenden sie standardmäßig AEM Host anstelle des SPA, was zu 404-Anfragen führt, wie in der Abbildung unten dargestellt.

![Beschädigte statische Ressourcen](./assets/spa-bootstrap/broken-static-resource.png)

Um dieses Problem zu beheben, stellen Sie sicher, dass eine statische Ressource, die von der Remote-SPA gehostet wird, absolute Pfade verwendet, die die Remote-SPA-Herkunft enthalten.

1. Öffnen Sie das SPA in Ihrer IDE.
1. Öffnen Sie die Datei SPA Umgebungsvariablen . `src/.env.development` und fügen Sie eine Variable für den SPA öffentlichen URI hinzu:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Bei der Bereitstellung auf AEM as a Cloud Service müssen Sie für die entsprechenden `.env` Dateien._

1. Öffnen Sie die Datei `src/App.js`
1. Importieren Sie den SPA öffentlichen URI aus den SPA Umgebungsvariablen

   ```
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. WKND-Logo voranstellen `<img src=.../>` mit `REACT_APP_PUBLIC_URI` , um eine Lösung gegen die SPA zu erzwingen.

   ```
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. Führen Sie dieselben Schritte aus, um Bilder in zu laden. `src/components/Loading.js`

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

1. Und für die __zwei Instanzen__ der Schaltfläche &quot;Zurück&quot;in `src/components/AdventureDetails.js`

   ```
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

Die `App.js`, `Loading.js`und `AdventureDetails.js` -Dateien sollten wie folgt aussehen:

![Statische Ressourcen](./assets/spa-bootstrap/static-resources.png)

## AEM responsives Raster

Um den Layout-Modus SPA Editors für bearbeitbare Bereiche im SPA zu unterstützen, müssen wir AEM CSS für responsives Raster in die SPA integrieren. Machen Sie sich keine Gedanken - dieses Rastersystem gilt nur für die bearbeitbaren Container und Sie können Ihr Rastersystem Ihrer Wahl verwenden, um das Layout des restlichen SPA zu steuern.

Fügen Sie die SCSS-Dateien AEM responsiven Rasters zum SPA hinzu.

1. Öffnen Sie das SPA in Ihrer IDE.
1. Laden Sie die beiden folgenden Dateien herunter und kopieren Sie sie in `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + Der SCSS-Generator AEM responsiven Rasters
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + Aufrufe `_grid.scss` Verwendung der SPA spezifischen Haltepunkte (Desktop und Mobilgerät) und Spalten (12).
1. Öffnen `src/App.scss` und importieren `./styles/grid-init.scss`

   ```
   ...
   @import './styles/grid-init';
   ...
   ```

Die `_grid.scss` und `_grid-init.scss` -Dateien sollten wie folgt aussehen:

![AEM Responsives Raster-SCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

Jetzt enthält der SPA das CSS, das erforderlich ist, um AEM Layout-Modus für Komponenten zu unterstützen, die einem AEM Container hinzugefügt werden.

## Starten Sie die SPA

Nachdem der SPA für die Integration mit AEM bootstrapping durchgeführt wurde, lassen Sie uns nun den SPA ausführen und sehen, wie er aussieht!

1. Navigieren Sie in der Befehlszeile zum Stammverzeichnis des SPA-Projekts.
1. Starten Sie den SPA mit den normalen Befehlen (falls noch nicht geschehen).

   ```
   $ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
   $ npm install 
   $ npm run start
   ```

1. Durchsuchen Sie die SPA auf [http://localhost:3000](http://localhost:3000). Alles sollte gut aussehen!

![SPA läuft auf http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## Öffnen Sie die SPA in AEM SPA Editor.

Mit laufendem SPA [http://localhost:3000](http://localhost:3000), öffnen wir es mit AEM SPA Editor. In der SPA ist noch nichts bearbeitbar, dies validiert nur die SPA in AEM.

1. Bei der AEM-Autoreninstanz anmelden
1. Navigieren Sie zu __Sites > WKND App > us > en__
1. Wählen Sie die __WKND-App-Startseite__ und tippen __Bearbeiten__ und der SPA angezeigt.

   ![WKND-App-Homepage bearbeiten](./assets/spa-bootstrap/edit-home.png)

1. Wechseln zu __Vorschau__ Verwenden des Modusschalters oben rechts
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

Nachdem wir nun eine Grundlinie der Kompatibilität mit AEM Editor erreicht haben, können wir damit beginnen, bearbeitbare Bereiche einzuführen. Wir schauen uns zunächst an, wie wir einen [feste bearbeitbare Komponente](./spa-fixed-component.md) im SPA.
