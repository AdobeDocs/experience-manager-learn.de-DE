---
title: SPA integrieren | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie der Quellcode für eine Einzelseiten-App (SPA), die in React geschrieben wurde, in ein Adobe Experience Manager (AEM)-Projekt integriert werden kann. Erfahren Sie, wie Sie mit modernen Frontend-Tools wie einem Webpack Development Server die SPA schnell mit der AEM JSON-Modell-API entwickeln können.
sub-product: Sites
feature: SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2107'
ht-degree: 2%

---


# Integrieren eines SPA {#integrate-spa}

Erfahren Sie, wie der Quellcode für eine Einzelseiten-App (SPA), die in React geschrieben wurde, in ein Adobe Experience Manager (AEM)-Projekt integriert werden kann. Erfahren Sie, wie Sie mit modernen Frontend-Tools wie einem Webpack Development Server die SPA schnell mit der AEM JSON-Modell-API entwickeln können.

## Vorgabe

1. Erfahren Sie, wie das SPA-Projekt in AEM mit Client-seitigen Bibliotheken integriert ist.
2. Erfahren Sie, wie Sie einen Webpack Development Server für die dedizierte Front-End-Entwicklung verwenden.
3. Erkunden Sie die Verwendung einer **proxy**- und statischen **mock**-Datei für die Entwicklung mit der AEM JSON-Modell-API.

## Was Sie erstellen werden

Dieses Kapitel fügt eine einfache `Header`-Komponente zum SPA hinzu. Bei der Erstellung dieser statischen `Header`-Komponente werden mehrere Ansätze für AEM SPA Entwicklung verwendet.

![Neue Kopfzeile in AEM](./assets/integrate-spa/final-header-component.png)

*Das SPA wird erweitert, um eine statische  `Header` Komponente hinzuzufügen.*

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Tutorial über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. Stellen Sie die Codebasis mithilfe von Maven in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das Profil `classic` hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `React/integrate-spa-solution` wechseln.

## Integrationsansatz {#integration-approach}

Im Rahmen des AEM wurden zwei Module erstellt: `ui.apps` und `ui.frontend`.

Das `ui.frontend`-Modul ist ein [webpack](https://webpack.js.org/)-Projekt, das den gesamten SPA Quellcode enthält. Ein Großteil der SPA wird im webpack-Projekt entwickelt und getestet. Wenn ein Produktions-Build ausgelöst wird, wird der SPA mithilfe des Webpack erstellt und kompiliert. Die kompilierten Artefakte (CSS und JavaScript) werden in das Modul `ui.apps` kopiert, das dann zur AEM Laufzeit bereitgestellt wird.

![ui.frontend-Architektur auf hoher Ebene](assets/integrate-spa/ui-frontend-architecture.png)

*Eine allgemeine Darstellung der SPA Integration.*

Weitere Informationen zum Front-End-Build finden Sie unter [hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect the SPA integration {#inspect-spa-integration}

Überprüfen Sie anschließend das Modul `ui.frontend` , um die SPA zu verstehen, die automatisch vom [AEM Projektarchetyp](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) generiert wurde.

1. Öffnen Sie in der IDE Ihrer Wahl das AEM Projekt für die WKND-SPA. In diesem Tutorial wird die [Visual Studio Code IDE](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code) verwendet.

   ![VSCode - AEM WKND SPA Project](./assets/integrate-spa/vscode-ide-openproject.png)

2. Erweitern und überprüfen Sie den Ordner `ui.frontend` . Öffnen Sie die Datei `ui.frontend/package.json`

3. Unter `dependencies` sollten Sie einige Verweise auf `react` sehen, darunter `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   `ui.frontend` ist eine React-Anwendung, die auf der [React-App erstellen](https://create-react-app.dev/) oder CRA basiert. Die `react-scripts`-Version gibt an, welche Version von CRA verwendet wird.

4. Es gibt auch drei Abhängigkeiten mit dem Präfix `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   Die oben genannten Module bilden das [AEM JS-SDK des Editors](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) und stellen die Funktionen bereit, mit denen SPA Komponenten AEM Komponenten zugeordnet werden können.

5. In der Datei `package.json` sind mehrere `scripts` definiert:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Dies sind standardmäßige Build-Skripte, die von der Create React App [available](https://create-react-app.dev/docs/available-scripts) bereitgestellt werden.

   Der einzige Unterschied besteht darin, dass `&& clientlib` zum `build`-Skript hinzugefügt wird. Diese zusätzliche Anweisung ist für das Kopieren der kompilierten SPA in das Modul `ui.apps` als Client-seitige Bibliothek während eines Builds verantwortlich.

   Das npm-Modul [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) wird verwendet, um dies zu erleichtern.

6. Prüfen Sie die Datei `ui.frontend/clientlib.config.js`. Diese Konfigurationsdatei wird von [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) verwendet, um zu bestimmen, wie die Client-Bibliothek generiert wird.

7. Prüfen Sie die Datei `ui.frontend/pom.xml`. Diese Datei wandelt den Ordner `ui.frontend` in einen Ordner [Maven-Modul](http://maven.apache.org/guides/mini/guide-multiple-modules.html) um. Die Datei `pom.xml` wurde aktualisiert, um das [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) zu **test** und **build** SPA während eines Maven-Builds zu verwenden.

8. Inspect die Datei `index.js` unter `ui.frontend/src/index.js`:

   ```js
   //ui.frontend/src/index.js
   ...
   document.addEventListener('DOMContentLoaded', () => {
       ModelManager.initialize().then(pageModel => {
           const history = createBrowserHistory();
           render(
           <Router history={history}>
               <App
               history={history}
               cqChildren={pageModel[Constants.CHILDREN_PROP]}
               cqItems={pageModel[Constants.ITEMS_PROP]}
               cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
               cqPath={pageModel[Constants.PATH_PROP]}
               locationPathname={window.location.pathname}
               />
           </Router>,
           document.getElementById('spa-root')
           );
       });
   });
   ```

   `index.js` ist der Einstiegspunkt des SPA. `ModelManager` wird vom AEM SPA Editor JS SDK bereitgestellt. Er ist für den Aufruf und die Injektion von `pageModel` (dem JSON-Inhalt) in die Anwendung verantwortlich.

## Hinzufügen einer Kopfzeilenkomponente {#header-component}

Fügen Sie anschließend eine neue Komponente zum SPA hinzu und stellen Sie die Änderungen auf einer lokalen AEM-Instanz bereit.

1. Erstellen Sie im Modul `ui.frontend` unter `ui.frontend/src/components` einen neuen Ordner mit dem Namen `Header`.
2. Erstellen Sie eine Datei mit dem Namen `Header.js` unter dem Ordner `Header` .

   ![Ordner und Datei der Kopfzeile](assets/integrate-spa/header-folder-js.png)

3. Füllen Sie `Header.js` wie folgt:

   ```js
   //Header.js
   import React, {Component} from 'react';
   
   export default class Header extends Component {
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           <h1>WKND</h1>
                       </div>
                   </header>
           );
       }
   }
   ```

   Oben ist eine standardmäßige React-Komponente, die eine statische Textzeichenfolge ausgibt.

4. Öffnen Sie die Datei `ui.frontend/src/App.js`. Dies ist der Einstiegspunkt der Anwendung.
5. Nehmen Sie die folgenden Aktualisierungen an `App.js` vor, um das statische `Header` einzuschließen:

   ```diff
     import { Page, withModel } from '@adobe/aem-react-editable-components';
     import React from 'react';
   + import Header from './components/Header/Header';
   
     // This component is the application entry point
     class App extends Page {
     render() {
         return (
         <div>
   +       <Header />
            {this.childComponents}
            {this.childPages}
        </div>
   ```

6. Öffnen Sie ein neues Terminal, navigieren Sie zum Ordner `ui.frontend` und führen Sie den Befehl `npm run build` aus:

   ```shell
   $ cd aem-guides-wknd-spa
   $ cd ui.frontend
   $ npm run build
   ...
   Compiled successfully.
   
   File sizes after gzip:
   
   118.95 KB (-33 B)  build/static/js/2.489f399a.chunk.js
   1.11 KB (+48 B)    build/static/js/main.6cfa5095.chunk.js
   806 B              build/static/js/runtime-main.42b998df.js
   451 B              build/static/css/main.e57bbe8a.chunk.css
   ```

7. Navigieren Sie zum Ordner `ui.apps`. Unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` sollten Sie sehen, dass die kompilierten SPA aus dem Ordner `ui.frontend/build` kopiert wurden.

   ![In ui.apps generierte Client-Bibliothek](./assets/integrate-spa/compiled-spa-uiapps.png)

8. Kehren Sie zum Terminal zurück und navigieren Sie zum Ordner `ui.apps` . Führen Sie den folgenden Maven-Befehl aus:

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   Dadurch wird das `ui.apps`-Paket auf einer lokalen laufenden Instanz von AEM bereitgestellt.

9. Öffnen Sie eine Browser-Registerkarte und navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Jetzt sollte der Inhalt der Komponente `Header` in der SPA angezeigt werden.

   ![Erste Kopfzeilenimplementierung](./assets/integrate-spa/initial-header-implementation.png)

   Die Schritte 6 bis 8 werden automatisch ausgeführt, wenn ein Maven-Build aus dem Stammverzeichnis des Projekts ausgelöst wird (d. h. `mvn clean install -PautoInstallSinglePackage`). Sie sollten jetzt die Grundlagen der Integration zwischen den SPA- und AEM Client-seitigen Bibliotheken verstehen. Beachten Sie, dass Sie weiterhin `Text`-Komponenten in AEM unter der statischen `Header`-Komponente bearbeiten und hinzufügen können.

## Webpack Dev Server - Proxy der JSON-API {#proxy-json}

Wie in den vorherigen Übungen gezeigt, dauert es einige Minuten, einen Build durchzuführen und die Client-Bibliothek mit einer lokalen Instanz von AEM zu synchronisieren. Dies ist für Endtests akzeptabel, aber nicht ideal für den Großteil der SPA.

Ein [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) kann verwendet werden, um die SPA schnell zu entwickeln. Die SPA wird von einem JSON-Modell gesteuert, das von AEM generiert wurde. In dieser Übung wird der JSON-Inhalt von einer laufenden Instanz von AEM **proxied** in den Entwicklungsserver übertragen.

1. Kehren Sie zur IDE zurück und öffnen Sie die Datei `ui.frontend/package.json`.

   Suchen Sie nach einer Zeile wie der folgenden:

   ```json
   "proxy": "http://localhost:4502",
   ```

   Die [React-App erstellen](https://create-react-app.dev/docs/proxying-api-requests-in-development) bietet einen einfachen Mechanismus zum Proxy von API-Anfragen. Alle unbekannten Anfragen werden über `localhost:4502`, den lokalen AEM-Schnellstart, bereitgestellt.

2. Öffnen Sie ein Terminal-Fenster und navigieren Sie zum Ordner `ui.frontend` . Führen Sie den Befehl `npm start` aus:

   ```shell
   $ cd ui.frontend
   $ npm start
   ...
   Compiled successfully!
   
   You can now view wknd-spa-react in the browser.
   
   Local:            http://localhost:3000
   On Your Network:  http://192.168.86.136:3000
   
   Note that the development build is not optimized.
   To create a production build, use npm run build.
   ```

3. Öffnen Sie eine neue Browser-Registerkarte (falls noch nicht geöffnet) und navigieren Sie zu [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Webpack-Dev-Server - Proxy-JSON](./assets/integrate-spa/webpack-dev-server-1.png)

   Sie sollten denselben Inhalt wie in AEM sehen, jedoch ohne eine der Authoring-Funktionen aktiviert zu haben.

   >[!NOTE]
   >
   > Aufgrund der Sicherheitsanforderungen von AEM müssen Sie in der lokalen AEM-Instanz (http://localhost:4502) im selben Browser, aber in einer anderen Registerkarte angemeldet sein.

4. Kehren Sie zur IDE zurück und erstellen Sie einen neuen Ordner mit dem Namen `media` unter `ui.frontend/src/media`.
5. Laden Sie das folgende WKND-Logo herunter und fügen Sie es zum Ordner `media` hinzu:

   ![WKND-Logo](./assets/integrate-spa/wknd-logo-dk.png)

6. Öffnen Sie `Header.js` unter `ui.frontend/src/components/Header/Header.js` und importieren Sie das -Logo:

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. Nehmen Sie die folgenden Änderungen an `Header.js` vor, um das -Logo als Teil der Kopfzeile einzuschließen:

   ```js
    export default class Header extends Component {
   
       get logo() {
           return (
               <div className="Logo">
                   <img className="Logo-img" src={wkndLogoDark} alt="WKND SPA" />
               </div>
           );
       }
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           {this.logo}
                       </div>
                   </header>
           );
       }
   }
   ```

   Speichern Sie die Änderungen in `Header.js`.

8. Kehren Sie zum Browser unter [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) zurück. Sie sollten die Änderungen an der App sofort sehen.

   ![Logo zur Kopfzeile hinzugefügt](./assets/integrate-spa/added-logo-localhost.png)

   Sie können weiterhin Inhaltsaktualisierungen in AEM vornehmen und sie in **webpack-dev-server** anzeigen, da wir den Inhalt proxieren.

9. Beenden Sie den Webpack-Dev-Server mit `ctrl+c` im Terminal.

## Webpack Dev Server - JSON-API nachahmen {#mock-json}

Ein weiterer Ansatz für die schnelle Entwicklung besteht darin, eine statische JSON-Datei zu verwenden, um als JSON-Modell zu fungieren. Durch &quot;nachahmen&quot;der JSON entfernen wir die Abhängigkeit von einer lokalen AEM-Instanz. Darüber hinaus kann ein Frontend-Entwickler das JSON-Modell aktualisieren, um die Funktionalität zu testen und Änderungen an der JSON-API vorzunehmen, die später von einem Back-End-Entwickler implementiert würden.

Die anfängliche Einrichtung der JSON-nachgeahmten Instanz erfordert **eine lokale AEM**.

1. Navigieren Sie im Browser zu [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Dies ist die von AEM exportierte JSON, die die Anwendung steuert. Kopieren Sie die JSON-Ausgabe.

2. Kehren Sie zur IDE zurück, navigieren Sie zu `ui.frontend/public` und fügen Sie einen neuen Ordner mit dem Namen `mock-content` hinzu.
3. Erstellen Sie eine neue Datei mit dem Namen `mock.model.json` unter `ui.frontend/public/mock-content`. Fügen Sie hier die JSON-Ausgabe aus **Schritt 1** ein.

   ![JSON-Modelldatei nachahmen](./assets/integrate-spa/mock-model-json-created.png)

4. Öffnen Sie die Datei `index.html` unter `ui.frontend/public/index.html`. Aktualisieren Sie die Metadateneigenschaft für das AEM Seitenmodell, um auf eine Variable `%REACT_APP_PAGE_MODEL_PATH%` zu verweisen:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   Die Verwendung einer Variablen für den Wert von `cq:pagemodel_root_url` erleichtert den Wechsel zwischen dem Proxy- und JSON-Modell.

5. Öffnen Sie die Datei `ui.frontend/.env.development` und nehmen Sie die folgenden Aktualisierungen vor, um den vorherigen Wert für `REACT_APP_PAGE_MODEL_PATH` zu kommentieren:

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. Wenn sie derzeit ausgeführt wird, stoppen Sie den **webpack-dev-server**. Starten Sie **webpack-dev-server** vom Terminal:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Navigieren Sie zu [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) und Sie sollten die SPA mit demselben Inhalt sehen, der in der JSON-Datei **proxy** verwendet wird.

7. Nehmen Sie eine kleine Änderung an der zuvor erstellten Datei `mock.model.json` vor. Sie sollten den aktualisierten Inhalt sofort im **webpack-Dev-Server** sehen.

   ![JSON-Update für Modell nachahmen](./assets/integrate-spa/webpack-mock-model.gif)

Die Fähigkeit, das JSON-Modell zu bearbeiten und die Auswirkungen auf eine Live-SPA zu sehen, kann Entwicklern dabei helfen, die JSON-Modell-API zu verstehen. Es ermöglicht auch die parallele Entwicklung von Frontend- und Backend-Anwendungen.

Sie können jetzt umschalten, wo der JSON-Inhalt verwendet werden soll, indem Sie die Einträge in der Datei `env.development` umschalten:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Stile mit Sass hinzufügen

Eine Best Practice von React besteht darin, jede Komponente modular und eigenständig zu halten. Es wird allgemein empfohlen, zu vermeiden, dass derselbe CSS-Klassenname über Komponenten hinweg wiederverwendet wird, was die Verwendung von Präprozessoren nicht so leistungsstark macht. Dieses Projekt verwendet [Sass](https://sass-lang.com/) für einige nützliche Funktionen wie Variablen. Dieses Projekt folgt auch locker [CSS-Namenskonventionen für SUIT-Dateien](https://github.com/suitcss/suit/blob/master/doc/components.md). SUIT ist eine Variante der BEM-Notation, Block Element Modifier, die zum Erstellen konsistenter CSS-Regeln verwendet wird.

1. Öffnen Sie ein Terminal-Fenster und stoppen Sie den **webpack-dev-server**, falls er gestartet wurde. Geben Sie im Ordner `ui.frontend` den folgenden Befehl in [install Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet) ein:

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   Installieren Sie `sass` als Peer-Abhängigkeiten:

   ```shell
   $ npm install sass --save
   ```

2. Installieren Sie `normalize-scss`, um die Stile über Browser hinweg zu normalisieren:

   ```shell
   $ npm install normalize-scss
   ```

3. Starten Sie **webpack-dev-server**, damit wir die Stile sehen können, die in Echtzeit aktualisiert werden:

   ```shell
   $ npm start
   ```

   Verwenden Sie entweder den Proxy of Mock-Ansatz zur Verarbeitung der JSON-Modell-API.

4. Kehren Sie zur IDE zurück und erstellen Sie unter `ui.frontend/src` einen neuen Ordner mit dem Namen `styles`.
5. Erstellen Sie eine neue Datei unter `ui.frontend/src/styles` mit dem Namen `_variables.scss` und fügen Sie sie mit den folgenden Variablen ein:

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

6. Benennen Sie die Erweiterung der Datei `index.css` unter `ui.frontend/src/index.css` in **`index.scss`** um. Ersetzen Sie den Inhalt durch Folgendes:

   ```scss
   /* index.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   //spacing for header
   body.page {
       padding-top: 75px;
   }
   ```

7. Aktualisieren Sie `ui.frontend/src/index.js`, um den neu benannten `index.scss` einzuschließen:

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. Erstellen Sie eine neue Datei mit dem Namen `Header.scss` unter `ui.frontend/src/components/Header`. Füllen Sie die Datei mit folgendem Inhalt:

   ```scss
   @import '../../styles/variables';
   
   .Header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .Logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .Logo-img {
       width: 100px;
   }
   ```

9. Fügen Sie `Header.scss` ein, indem Sie `Header.js` aktualisieren:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. Kehren Sie zum Browser und zum **webpack-dev-server** zurück: [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Styled Header - webpack Development Server](./assets/integrate-spa/styled-header.png)

   Sie sollten nun die aktualisierten Stile sehen, die der Komponente `Header` hinzugefügt wurden.

## Bereitstellen SPA Updates für AEM

Die Änderungen, die an `Header` vorgenommen wurden, sind derzeit nur über den **webpack-Dev-Server** sichtbar. Stellen Sie die aktualisierte SPA für AEM bereit, um die Änderungen anzuzeigen.

1. Navigieren Sie zum Stammverzeichnis des Projekts (`aem-guides-wknd-spa`) und stellen Sie das Projekt mithilfe von Maven für AEM bereit:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Sie sollten das aktualisierte `Header` mit angewendetem Logo und angewendeten Stilen sehen.

   ![Aktualisierte Kopfzeile in AEM](./assets/integrate-spa/final-header-component.png)

   Nachdem sich die aktualisierte SPA in AEM befindet, kann die Bearbeitung fortgesetzt werden.

## Fehlerbehebung für den Fehler node-sass

Bei der Entwicklung kann der folgende Fehler auftreten:

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

Dies kann vorkommen, wenn sich die lokale Version von **Node.js** und **npm** von der vom [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) verwendeten unterscheidet. Wenn Sie den Befehl `npm rebuild node-sass` ausführen, kann das Problem vorübergehend behoben oder der Ordner `ui.frontend/node_modules` entfernt und neu installiert werden.

Es gibt auch einige Möglichkeiten, dieses Problem nachhaltiger anzugehen.

* Stellen Sie sicher, dass die lokale Version von npm und Node.js mit den Versionen übereinstimmt, die vom [Maven-Build](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118) verwendet werden.
* Fügen Sie dem Schritt `ui.frontend/pom.xml` vor dem Schritt `npm run build` den folgenden Ausführungsschritt hinzu:

   ```xml
   <execution>
       <id>npm rebuild node-sass</id>
       <goals>
           <goal>npm</goal>
       </goals>
       <configuration>
           <arguments>rebuild node-sass</arguments>
       </configuration>
   </execution>
   ```

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben die SPA aktualisiert und die Integration mit AEM untersucht! Sie kennen jetzt zwei verschiedene Ansätze zur Entwicklung des SPA mit der AEM JSON-Modell-API mit einem **webpack-dev-server**.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `React/integrate-spa-solution` wechseln.

### Nächste Schritte {#next-steps}

[Zuordnen SPA Komponenten zu AEM Komponenten](map-components.md)  - Erfahren Sie, wie Sie React-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM)-Komponenten zuordnen. Die Komponentenzuordnung ermöglicht es Benutzern, im AEM SPA Editor dynamische Aktualisierungen an SPA -Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM.
